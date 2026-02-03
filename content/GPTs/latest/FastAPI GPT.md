---
description: A custom GPT coding assistant, equiped with knowledge of the latest FastAPI features and smart presets.
draft: false
tags:
  - ArtificialIntelligence
  - ChatGPT
  - customGPTs
enableToc: true
unlisted: false
title: "[latest] FastAPI GPT"
date: 2023-11-22
---
<img src="https://raw.githubusercontent.com/luona-dev/latestGPTs/main/assets/fastapi/logo_fastapigpt.png" width="96" alt="Icon of the FastAPI GPTs" style="float: left; margin-right: 10px;">
Custom GPT coding assistants, equiped with knowledge of the latest FastAPI features and smart presets. 

## Available Versions
---
title: Tailwind CSS GPT from [latest] GPTs family
description: A custom GPT coding assistant, equiped with knowledge of the latest Tailwind CSS features and smart presets.
layout: base
date: 2023-12-04
---
# [latest] Tailwind CSS GPT

<img src="/assets/gpts/tailwind/logo_tailwindgpt.webp" width="96" alt="Icon of the Tailwind GPTs" style="float: left; margin-right: 10px;">
An up-to-date, state-of-the-art coding assistant for Tailwind CSS, the utility-first CSS framework. As [all our coding assistants](/gpts/), it is equiped with curated knowledge of the latest Tailwind CSS features and smart presets. 

<br>
## Available Versions

| GPT | Version | Last Update📅 | Notes🗒️ |
|---|---|---|---|
| [[latest] Tailwind CSS GPT](https://chat.openai.com/g/g-qrreXSScH-latest-tailwind-css-gpt) | 3.4.1 | 2024-01-08 | 😍 |
| [[3.3.7] Tailwind CSS GPT](https://chat.openai.com/g/g-qPiMDcJT7) | 3.3.7 | 2023-12-18 | |

## Presets

The coding assistant is configured to be a concise and meticulous Web developer that adheres to latest standards and best practices. It should optimize for performance and accessibility, DRY code and modern CSS features. For now it does not make use of any conversation starters to customize developer experience, but feel free to let us know if there is anything we should add to the presets by [opening an issue](/README.md#contribution-guidelines).

## Tests & Performance 🌡️

### Version 3.4.0

#### 🧪 `has-*`, `group-has-*`, and `peer-has-*`

- **Difficulty** : 🟠 Medium
- **Prompt**: `I want to style a parent container based on its child. Is that posiible?`
- **Expected Outcome**: `Yes with the new :has() pseudo-class`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ | Sometimes gives a not working example of the has-* variant  |
| 3.3.7   | ❌ |   |

#### 🧪 `text-wrap` utilities

- **Difficulty** : 🟠 Medium
- **Prompt**: `I don't want to have a single word on a line in my headlines. How can I prevent that?`
- **Expected Outcome**: `Use the new text-wrap balanced utility`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| 3.3.7   | ❌ |   |

#### 🧪 `size-` utilities

- **Difficulty** : 🟢 Easy
- **Prompt**: `How can I set the width and height of an element at the same time?`
- **Expected Outcome**: `Use the size-* utility`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| 3.3.7   | ❌ |   |

- **Difficulty** : 🔴 Hard
- **Prompt**: `I need a square element. How can I achieve that?`
- **Expected Outcome**: `Use the size-* utility instead of w-* and h-*`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| 3.3.7   | ❌ |   |

#### 🧪 direct child selector

- **Difficulty** : 🟢 Easy
- **Prompt**: `How can I style the children of an element?`
- **Expected Outcome**: `Use the * variant`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| 3.3.7   | ❌ |   |

- **Difficulty** : 🟠 Medium
- **Prompt**: `I have a list of <div> elements within a container and want to style them all at once. How can I do that?`
- **Expected Outcome**: `Use the * variant to style all direct children of the`

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| 3.3.7   | ❌ |   |


### Version 3.3.3

Tests will follow.

| GPT                                                                                     | Version | Last Update📅 | Notes🗒️                  |
| --------------------------------------------------------------------------------------- | ------- | ------------- | ------------------------- |
| [[latest]FastAPI GPT](https://chat.openai.com/g/g-BhYCAfVXk-latest-fastapi-gpt)         | 0.108.0 | 2024-01-05    | 😍                        |
| [[l]FastAPI GPT- plain](https://chat.openai.com/g/g-UXYVWTbat-latest-fastapi-gpt-plain) | ✖️      | 2023-11-22    | for testing purposes only |

## Presets

The coding assistant is a concise and meticulous Python developer that adheres to latest standards and best practices.
It should make excessive use of type hints and annotations. It is told to use Pydantic v2 features and sqlalchemy 2.0 with the new Typing API.
If you do not like these settings, or there are any other presets that you find yourself using over and over again, please let us know, by [opening an issue](https://github.com/luona-dev/latestGPTs?tab=readme-ov-file#contribution-guidelines).

## Tests & Performance 🌡️

We want to create a comprehensive test suite for the **\[latest] GPTs** to ensure their quality and their knowledge retrieval abilities.
The tests are sorted by version and focus on specific features/deprecations that came with that version. Each test consists of a tasks, that an up-to-date Vue.js coding assistant should master. [Contributions](https://github.com/luona-dev/latestGPTs?tab=readme-ov-file#contribution-guidelines) are very welcome!

As a comparison, each test is also conducted with "plain GPT" a GPT that has the same instructions as our **\[latest] GPT** but no access to a knowledge file.

### Version 0.89.0

#### 🧪 return type annotations to declare the response_model

- **Prompt:** `Can OpenAPI deduce the response_model from the return type annotation?`
- **Difficulty:** 🟢 Easy
- **Expected Outcome:** `Yes + a short explanation of the functionality`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |

---
- **Prompt:** `Show me an example route that returns a object of type Item(BaseModel).`
- **Difficulty:** 🔴 Hard
- **Expected Outcome:** `The usage of a return type annotation instead of using the response_model parameter`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ❌ | Often suggests to use a return type, but it semms random and unfounded.  |
| plain GPT | ❌ |   |

---
### Version 0.93.0

#### 🧪 lifespan async context managers

- **Prompt:** `I have some heavy computation to do once for my entire application. How can I do that?`
- **Difficulty:** 🟡 Medium
- **Expected Outcome:** `suggest @asynccontextmanager and lifespan events instead of startup and shutdown`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |

---

### Version 0.95.0

#### 🧪 Usage of Annotated

- **Prompt:** `How do I declare dependencies in my FastAPI route?`
- **Difficulty:** 🟢 Easy
- **Expected Outcome:** `something like async def handler(dep: Annotated[MyClass, Depends(my_dependency)]): instead of default value syntax`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |

---


### Version 0.100.0

#### 🧪 Usage of model_config instead of class `Config` in pydantic models

- **Prompt:** `How can I add an example to use with OpenAPI for my Pydantic model?`
- **Difficulty:** 🟢 Easy
- **Expected Outcome:** `usage of a model_config dictionary instead of a class Config`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |

---

### Version 0.101.0

#### 🧪 Usage of computed_field in pydantic models

- **Prompt:** `How can I add a computed field to my Pydantic model?`
- **Difficulty:** 🟢 Easy
- **Expected Outcome:** `usage of the computed_field decorator`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |


- **Prompt:** `I need a Pydantic model with a field that is based on another field. It is fairly computationally heavy. How can I do that?`
- **Difficulty:** 🟡 Medium
- **Expected Outcome:** `usage of the computed_field decorator`

**Results:**

| Version | Pass | Notes |
|---|---|---|
| latest | ✅ |   |
| plain GPT | ❌ |   |

