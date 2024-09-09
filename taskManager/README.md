# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).

------

Task Manager App built for a technical VueJS test

I initially wanted to use Ubuntu server then changed to Windows 11


------

PREPARE THE ENVIRONMENT

I installed Node.js through the .msi package on https://nodejs.org/en/download/package-manager

After restarting the computer, I checked the version 
> npm -v

CREATE VITE PROJECT

>npm create vite@latest nameoftheproject

√ Package name: ... nameoftheproject
√ Select a framework: » Vue
√ Select a variant: » JavaScript

Done. Now run:

  cd nameoftheproject
  npm install
  npm run dev

---------

PROJECT STRUCTURE

task-manager/
 index.html
  src/
   main.js
   App.vue
   components/
      TaskForm.vue
      TaskList.vue
      TaskFilter.vue
  package.json
  vite.config.js



