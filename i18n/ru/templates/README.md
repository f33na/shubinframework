# templates/

Файлы, которые команда, внедряющая фреймворк, копирует в свои репозитории.

Shubin Framework — это документация, не CLI и не scaffolder. Нет команды `init` и нет реестра шаблонов. Когда фреймворк предписывает конкретный файл — Issue Template, `AGENTS.md` для конкретной директории, git-конвенцию — канонический вариант этого файла лежит здесь, а команда копирует его в нужное место в своём репозитории.

Текущее содержимое:

- `.github/ISSUE_TEMPLATE/` — шаблоны GitHub Issues плюс `config.yml` и copy-гайд. Две группы:
  - Для **код-репозиториев**: `feature.md` (обязателен для P0/P1 single-part фич), `feature-light.md` (P2 и мелкая работа), `bug.md`, `sub-issue.md`. См. `framework.md` гл. 3.2 — что обеспечивает Feature template и почему.
  - Для **markdown-репозитория (vault)**: `cross-cutting.md` (основной Issue для фич, затрагивающих 2+ части), `vault-task.md` (работа только в документации), `adr-initiative.md` (крупные ADR-инициативы), `retro-item.md` (follow-up из ретроспектив). Правила размещения — `framework.md` гл. 3.3 (Where Issues live) и 3.4 (Cross-cutting features).
- `knowledge/AGENTS.md` — инструкции для AI-агента, который поддерживает `knowledge/wiki/`. Кладётся в markdown-репозиторий команд, подключивших опциональный knowledge-модуль. Общую картину, включая workflow auto-ingest и интеграцию с Projects, см. в `knowledge.md`.
- `conventions/VAULT_GIT_CONVENTIONS.md` — соглашение о git-коммитах для markdown-репозитория vault. Отличается от базовой конвенции, используемой в код-репозиториях: vault-специфичные теги (ADR, ARCH, JOURNAL, WIKI-AGENT, ...) и без bump-ов версий.

Шаблоны намеренно не auto-install. Копирование — осознанное действие: команда должна прочитать файл, понять его и адаптировать под свои названия и конвенции до коммита.
