# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).

# 📹 Vue Mobile Video Recorder

Стабільний Vue 3 компонент для запису відео з камери на мобільних пристроях через браузер.

---

## 🚀 Опис

Цей компонент дозволяє запускати камеру мобільного пристрою, вести запис відео через API `MediaRecorder`, переглядати результат та завантажувати відео.

Компонент розроблений спеціально для мобільних браузерів з урахуванням підтримки iOS та Android, з перевіркою MIME-типів, обробкою помилок та повторною ініціалізацією камери.

---

## ✅ Функціонал

- Запуск задньої камери (`facingMode: 'environment'`).
- Перевірка підтримки MIME-типів відео перед стартом запису.
- Запис відео через `MediaRecorder`.
- Перегляд записаного відео після зупинки запису.
- Завантаження відео-файлу на пристрій.
- Повторний запис без перезавантаження сторінки.
- Обробка помилок (відсутність камери, MediaRecorder, підтримки браузера).

---

## ⚙️ Встановлення

```bash
npm install git+https://github.com/yourname/vue-mobile-recorder.git
