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
  - Edit this file to reflect your actual project goals, market context, etc.

----------
**Very Important**: When Gemini asks your permission to create/change or write to files, make sure you select **allow once** only. Never give full permissions to an AI to make changes to your file system. NEVER.

----------
