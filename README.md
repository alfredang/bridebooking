# Aurelia Bridal

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Google Fonts](https://img.shields.io/badge/Google%20Fonts-4285F4?style=for-the-badge&logo=googlefonts&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=for-the-badge&logo=githubpages&logoColor=white)

![Aurelia Bridal — site preview](screenshot.png)

## About

A single-page marketing and booking site for **Aurelia Bridal Photography** — bespoke bridal sessions, engagement shoots, and full-day wedding coverage in soft, romantic light. The whole site is one `index.html` (markup, inline `<style>`, inline `<script>`) with no build step or framework. Bookings are submitted to a [FormSubmit](https://formsubmit.co/) endpoint that emails the studio when a couple requests a session.

## File structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml
├── .gitignore
├── CLAUDE.md
├── README.md
└── index.html
```

## How to use

```bash
git clone https://github.com/alfredang/bridebooking.git
cd bridebooking
python -m http.server 8080
```

Then open <http://localhost:8080/>.

The booking form posts JSON to a remote FormSubmit endpoint, so it needs an HTTP origin (not `file://`) to work end-to-end. For visual-only changes, opening `index.html` directly is fine.

> **Heads-up — FormSubmit first-time activation:** the very first booking POST to a new recipient address triggers a one-time confirmation email from FormSubmit. Until that link is clicked, no submissions deliver. This is expected.

## Live site

🌐 **Live site:** https://alfredang.github.io/bridebooking/
