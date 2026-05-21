# Discussion Log — 01 Add Dark Mode

Area: Estratégia de tema
Date: 2026-05-21

Questions and answers:

1. Estratégia de tema — escolha: `Tailwind 'dark' class`.
2. Comportamento padrão — escolha: `Sempre iniciar em claro (light)`.
3. Hydration/SSR — escolha: `Pequeno script inline (client) que lê localStorage`.
4. Config Tailwind — escolha: `Sim — ativar darkMode: 'class'`.

Notes: User selected a class-based Tailwind approach and prefers starting in light mode. Hydration will use localStorage-based inline script to avoid FOUC.
