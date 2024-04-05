# Sherra Hackathon - Web APP

## Description
Secured first place by developing a VueJS-based web application powered by the OpenAI API, empowering entrepreneurs to efficiently transform their ideas into actionable plans. Utilizing Generative AI, the application generates comprehensive business plans with just a click, simplifying the process for entrepreneurs.

## Demo
https://still-thinking-25504.web.app



## Setup

1. Clone this repository and navigate into the project directory

    ```shell
    git clone https://github.com/zay0od/Sherra-Hackathon---Web-App.git
    ```

2. Using `pnpm` to install dependencies
  
    ```shell
    pnpm install
    ```

3. Create the `.env` file to store your OpenAI API key and Orgnaization ID

    ```shell
    touch .env
    ```

4. Add your OpenAI API key and Orgnaization ID to the `.env` file

    ```shell
    # Example
    VITE_OPEN_API_KEY=xxxxxxxx
    VITE_ORG_ID=org-xxxxxxx
    ```

5. Run the development server, and open [http://localhost:5173](http://localhost:5173) in your browser

    ```shell
    pnpm dev
    ```

6. Build the project for production

    ```shell
    pnpm build
    ```
