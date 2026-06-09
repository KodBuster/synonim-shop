# СИНОНИМ — бренд-токены для storefront (Next.js 15)

Источник: «Краткая информация по логотипу Синоним.pdf».
Концепция: современная демократичная ювелирка, эко-технологичность с женственным акцентом. ЦА — девушки 20–40. Знак «О» = круглая огранка бриллианта.

> ⚠️ В PDF два набора hex для оливкового/терракотового (стр. 3 vs стр. 5). Ниже: яркие значения (стр. 3, с Pantone) = бренд-палитра; тёмные (стр. 5) = функциональные UI-оттенки. `bg` и `ink` — производные, в брендбуке их нет, добавлены для рабочего UI. Подтвердить «главный» оливковый у дизайнера.

## Палитра

| Роль | Token | HEX | Источник | Назначение |
|---|---|---|---|---|
| Бренд / оливковый | `brand` | `#73A341` | стр. 3, Pantone 369 C | логотип, бренд-акценты, иконки |
| Тёмный оливковый | `brand-dark` | `#4A5335` | стр. 5 | текст-акцент, кнопки (белый текст проходит AA), хедер/футер |
| Акцент / терракотовый | `accent` | `#D47750` | стр. 3, Pantone 7590 C | ленты, детали, hover, бейджи |
| Тёмный терракотовый | `accent-deep` | `#C96A4A` | стр. 5 | акцент на светлом, иконки |
| Песочный | `sand` | `#E8D9B5` | стр. 3+5, Pantone 468 C | мягкие подложки, секции, карточки |
| Фон (производный) | `bg` | `#FBF8F1` | — | тёплый кремовый фон страниц (как в макетах PDF) |
| Чистый фон | `surface` | `#FFFFFF` | — | карточки товара, поля ввода |
| Текст (производный) | `ink` | `#1A1A1A` | — | основной текст на светлом |
| На тёмном | `on-dark` | `#FFFFFF` | стр. 5 | текст/лого на тёмных и насыщенных фонах |

Доступность: яркий `brand`/`accent` на белом дают контраст ~2.6–2.9:1 → **не для мелкого текста**. Для CTA: фон `brand-dark` + белый текст (AA ок), либо фон `accent` + тёмный текст `ink`.

## globals.css

```css
:root {
  /* Бренд */
  --color-brand: #73A341;
  --color-brand-dark: #4A5335;
  --color-accent: #D47750;
  --color-accent-deep: #C96A4A;
  --color-sand: #E8D9B5;

  /* Поверхности и текст */
  --color-bg: #FBF8F1;
  --color-surface: #FFFFFF;
  --color-ink: #1A1A1A;
  --color-on-dark: #FFFFFF;

  /* Типографика */
  --font-display: "Playfair Display", Georgia, serif;
  --font-body: "Inter", system-ui, sans-serif;

  /* Радиусы/тени — нейтральные, под «премиальную доступность» */
  --radius: 4px;
  --shadow-card: 0 1px 3px rgba(74, 83, 53, 0.08);
}

body { background: var(--color-bg); color: var(--color-ink); font-family: var(--font-body); }
h1, h2, h3 { font-family: var(--font-display); color: var(--color-brand-dark); }
```

## Tailwind v4 (@theme в globals.css)

```css
@import "tailwindcss";

@theme {
  --color-brand: #73A341;
  --color-brand-dark: #4A5335;
  --color-accent: #D47750;
  --color-accent-deep: #C96A4A;
  --color-sand: #E8D9B5;
  --color-bg: #FBF8F1;
  --color-surface: #FFFFFF;
  --color-ink: #1A1A1A;

  --font-display: "Playfair Display", Georgia, serif;
  --font-body: "Inter", system-ui, sans-serif;
}
```

Использование: `bg-brand`, `text-brand-dark`, `bg-accent`, `bg-sand`, `font-display`, `font-body`.

## Шрифты — next/font (app/fonts.ts)

```ts
import { Playfair_Display, Inter } from "next/font/google";

export const playfair = Playfair_Display({
  subsets: ["latin", "cyrillic"],
  weight: ["400", "600", "700"],
  variable: "--font-display",
  display: "swap",
});

export const inter = Inter({
  subsets: ["latin", "cyrillic"],
  weight: ["400", "500", "600"],
  variable: "--font-body",
  display: "swap",
});
```

В `app/layout.tsx`: `<html className={`${playfair.variable} ${inter.variable}`}>`.

- **Playfair Display** — заголовки (serif, премиальность).
- **Inter** — основной текст (экранная читаемость). Оба поддерживают кириллицу.

## Логотип — правила для веба

- Версии: цветной (основной) · чёрный (на светлом) · белый (на тёмном/насыщенном).
- Мин. размер на экране: **80px по ширине** (для печати — 20 мм).
- Защитное поле: свободное пространство вокруг ≥ высоты знака «О». Внутри ничего не размещать.
- Фон: светлый (песочный/белый) или контрастный нейтральный. На пёстром/насыщенном — только одноцветная версия.
- Нельзя: менять пропорции, добавлять тени/градиенты/обводки, перекрашивать, наклонять/вращать/обрезать.
- Файлы: держать SVG для всех трёх версий (масштабируемость + чёткость на ретине).
