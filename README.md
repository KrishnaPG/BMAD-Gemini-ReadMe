# BMAD-Gemini-ReadMe
The step by step instructions on how to use BMAD with Gemini-Cli are laid out below. 
  - If you are lost at anytime, you can use this custom ChatGPT for help: [BMAD Prompt Coach GPT](https://chatgpt.com/g/g-68762edf94388191a94f75531cba55fc-bmad-prompt-coach).

## 1. Install Gemini-CLI
Gemini-Cli needs to be installed once and can be done by following the [instructions](https://github.com/google-gemini/gemini-cli); 
```sh
npm install -g @google/gemini-cli
```
Then you can run the CLI from anywhere using the command:
```sh
gemini
```

## 2. Install the BMAD
For each project you need to run the BMAD install afresh separately.
  - BMAD needs to be run inside a project folder. Create a new project folder and switch to that directory in the command prompt:
    ```sh
      mkdir myProject
      cd myProject
    ```
  - From inside the project directory, run the below command to initialize the BMAD setup:
    ```sh
      npx bmad-method install
    ```
    Follow the instructions there-in and choose all defaults. When asked for the project directory path, on Windows give the windows style paths (e.g. `G:\Codebase\MyProject` );
    
## 3. Setup Gemini Auth
Each Project can have its own Gemini API Key. The best way is to create a `.env` file in the project directory with the content as below:
  ```.env
  GEMINI_API_KEY=AIzaSy...tz8
  ```
You can obtain the Gemini API Key by following instructions at: [Get Gemini API Key](https://goo.gle/gemini-cli-docs-auth#gemini-api-key) if you do not have one ready.

## 4. Launch Gemini CLI
Run from inside the project directory the below command (make sure the `.env` file with an API key is already present in that project folder):
  ```sh
    gemini
  ```
When prompted for authentication, choose 'use Gemini API Key` option. If not prompted and you want to change the preferred authentication, type `/auth` at the Gemini CLI and it should prompt you.

## 5. Initialize the Project brief
The project brief needs to be in a specified format for the BMAD to work. In the **Gemini CLI**, enter the below prompt:
  ```
    @analyst *create-doc project-brief-tmpl.yaml ./docs/brief.md
  ```
  - This creates a structured `docs/brief.md` with required sections.
  - **Manually** edit this file to reflect your actual project goals, market context, etc. without changing its structure.

----------
**Very Important**: When Gemini asks your permission to create/change or write to files, make sure you select **allow once** only. Never give full permissions to an AI to make changes to your file system. NEVER.

----------

## 6. Generate PRD from Brief
Once the project brief is edited manually to suit your needs, in the **Gemini CLI**, enter the below prompt:
  ```
    @pm *create-doc prd-tmpl.yaml ./docs/prd.md 
  ```
  - This uses the brief as input (automatically loaded by @pm).
  - Produces a full `docs/prd.md` with functional & non-functional requirements, epics, and assumptions scaffolded.
  - **Manually** edit it as you need, without changing its structure.

## 7. Architecture
Once the PRD is edited manually to suit your needs, in the **Gemini CLI**, enter the below prompt:
  ```
    @architect *create-doc fullstack-architecture-tmpl.yaml ./docs/architecture.md
  ```
Alternately, the PRD may also contain the next steps and the prompts to run the architect.
  - This uses the PRD as input and produces the `docs/architecture.md`.
  - **Manually** edit it as you need, without changing its structure.

## 8. Shard the PRD and Architecture
After the Architect has created the `docs/architecture.md` and you have manually edited to suit your needs, in the **Gemini CLI**, enter the below prompts:
  ```
    @bmad-master *shard-doc docs/prd.md docs/prd
  ```
followed by
  ```
    @bmad-master *shard-doc docs/architecture.md docs/architecture 
  ```
Check that these below files exist and are meaningful:
```
  docs/architecture/coding-standards.md
  docs/architecture/tech-stack.md
  docs/architecture/source-tree.md
```
----------
**Safe Usage Tips**:

  - ❌ Do not re-run the `@architect *create-doc ...` command unless you are okay with losing edits in architecture.md.

  - ✅ If you want to regenerate a missing section in the `docs/architecture.md`, it's better to copy from the template manually or restore specific parts.

  - ✅ If you are just missing devLoadAlwaysFiles (configured in `core-config.yaml`), manually create only those files by copying from the [templates](templates/) folder in this repo.


----------

## 9. Validate the Architecture Document
Optionally, you can run the below prompt in the **Gemini CLI** to have architect do validation of the architecture. 
  ```
   @architect *execute-checklist architect-checklist.md
  ```
  - The above command generates a report, but by default it does NOT **save to disk**.
  - You have to manuallly enter a prompt to save the report to disk in a path similar to: `docs/validation/architect-checklist-2025-07-23.md`
  - Commit it to Git for future audits:
    ```
      git add docs/validation/architect-checklist-report-2025-07-23.md
      git commit -m "Add architecture validation checklist report"
    ```
  - Next, you can ask the Architect to work on the checklist report to resolve any pending items, using the below prompt:
    ```
    @architect Review the checklist report at docs/validation/architect-checklist-2025-07-23.md and fill any unresolved ❌ items by updating the appropriate architecture files. Confirm each update in the same report.
    ```

## 10. Initiate Development: SM → Dev → QA loop
  - **Create the First Story** (use new Gemini chat session for clean context):
    ```
      @sm *create-next-story ./ docs/stories/story-1.0.md  # generate first story from PRD + architecture
    ```
    or alternately you can skip the filename and the next one in sequence will be automatically generated:
    ```
      @sm *create-next-story ./ docs/stories/  # auto-generate next story in sequence
    ```
  - **Approve the Story**:
    - Manually review story file (e.g. `docs/1.1.story.md`)
    - Update status from `Draft` → `Approved`
   
    Additionally the created story can be validated with below prompt:
    ```
      @po *validate-next-story docs/stories   # PO approves next story for development
    ```
  - **Develop the Story** (new chat session):
    ```
      @dev Implement this story docs/stories/1.1.story.md 
    ```
  - **Run QA Review** (new chat session):
    ```
      @qa *review-story docs/stories/1.1.story.md
    ```
    After QA verifies and approves the code, repeat the cycle (in a new chat sessions) by creating next story.
  - Monitor `docs/stories/` for progress tracking and status alignment

## 11. Continue Implementation after a manual code overwrite
  - If you do not like what the `dev` produced and rather manually edited something, then use the below **GeminiCLI prompt** to continue:
    ```
      @dev Continue implementing this story docs/stories/1.1.story.md from the current project state   # Preserves manual edits
    ```
