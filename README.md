# Editable Account Panel

A modern, responsive account panel built with Vue 3 and Vite. Users can view and edit their profile information, including name, email, and password, with real-time validation, password strength feedback, and Gravatar integration.

---

## 🛠 Tech Stack

- **Vue 3** — Progressive JavaScript framework for building user interfaces
- **Vite** — Next-generation frontend tooling for fast development and builds
- **SCSS (Sass)** — CSS preprocessor for modular and maintainable styles
- **Font Awesome** — Icon library for UI icons
- **zxcvbn** — Password strength estimation library
- **blueimp-md5** — MD5 hashing for Gravatar integration
- **gh-pages** — For deploying the app to GitHub Pages

---

## ⚙️ How It Works

- **Profile Editing:**  
  Users can edit their name, email, and password. Validation ensures all fields are filled and the email is valid.
- **Password Strength:**  
  As users type a new password, a strength meter (powered by zxcvbn) provides feedback.
- **Gravatar Integration:**  
  The user's avatar is fetched from Gravatar based on their email address.
- **Toast Notifications:**  
  Success and error messages are shown as toasts, with fade-out animation.
- **Responsive UI:**  
  Styled with SCSS for a clean, modern look and responsive layout.

---

## 📦 Prerequisites

- [Node.js](https://nodejs.org/) (v16 or higher recommended)
- [npm](https://www.npmjs.com/) (comes with Node.js)

---

## 🚀 Getting Started

1. **Clone the repository:**
    ```bash
    git clone https://github.com/<your-username>/editable-account-panel.git
    cd editable-account-panel
    ```

2. **Install dependencies:**
    ```bash
    npm install
    ```

3. **Run the development server:**
    ```bash
    npm run dev
    ```

4. **Build for production:**
    ```bash
    npm run build
    ```

5. **Preview the production build:**
    ```bash
    npm run preview
    ```

---

## 🌐 Deploying to GitHub Pages

1. **Set the base path in `vite.config.js`:**
    ```js
    import { defineConfig } from 'vite'
    import vue from '@vitejs/plugin-vue'

    export default defineConfig({
      plugins: [vue()],
      base: '/editable-account-panel/' // Use your repo name
    })
    ```

2. **Deploy:**
    ```bash
    npm run deploy
    ```

3. **Visit:**  
   `https://<your-username>.github.io/editable-account-panel/`

---

## 📁 Project Structure

```
editable-account-panel/
├── src/
│   ├── components/
│   │   └── AccountPanel.vue
│   ├── App.vue
│   ├── main.js
│   └── style.scss
├── public/
├── package.json
├── vite.config.js
└── README.md
```

---

## 🙏 Credits

- [Vue.js](https://vuejs.org/)
- [Vite](https://vitejs.dev/)
- [Font Awesome](https://fontawesome.com/)
- [zxcvbn](https://github.com/dropbox/zxcvbn)
- [blueimp-md5](https://github.com/blueimp/JavaScript-MD5)

---

## 📃 License

MIT

---
