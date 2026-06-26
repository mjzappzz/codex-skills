# Codex Skills

Codex 自定义 Skill 集合，当前共 **39** 个技能目录。

每个 Skill 是一个独立目录，入口文件为 `SKILL.md`，可按任务按需加载。

## 快速开始

```bash
git clone git@github.com:mjzappzz/codex-skills.git ~/.codex/skills
find ~/.codex/skills -maxdepth 2 -name SKILL.md | sort
```

## 目录结构

```text
~/.codex/skills/
├── api-and-interface-design/
├── browser-testing-with-devtools/
├── ci-cd-and-automation/
├── code-review-and-quality/
├── code-simplification/
├── context-engineering/
├── debugging-and-error-recovery/
├── deprecation-and-migration/
├── diagnose/
├── documentation-and-adrs/
├── doubt-driven-development/
├── frontend-ui-engineering/
├── git-workflow-and-versioning/
├── grill-with-docs/
├── gsap-core/
├── gsap-frameworks/
├── gsap-performance/
├── gsap-plugins/
├── gsap-react/
├── gsap-scrolltrigger/
├── gsap-timeline/
├── gsap-utils/
├── idea-refine/
├── incremental-implementation/
├── interview-me/
├── observability-and-instrumentation/
├── performance-optimization/
├── planning-and-task-breakdown/
├── security-and-hardening/
├── setup-matt-pocock-skills/
├── shipping-and-launch/
├── source-driven-development/
├── spec-driven-development/
├── stress-report/
├── tdd/
├── test-driven-development/
├── ui-ux-pro-max/
├── using-agent-skills/
└── zoom-out/
```

## Skills 一览

| Skill | 简述 | 路由说明 |
|-------|------|----------|
| `api-and-interface-design` | API 与接口设计 | Guides stable API and interface design. Use when designing APIs, module boundaries, or any public interface. Use when creating REST or GraphQL endpoints, defining type contracts between modules, or establishing boundaries between frontend and backend. |
| `browser-testing-with-devtools` | 浏览器测试与调试 | Tests in real browsers via Chrome DevTools MCP. Use when building or debugging anything that runs in a browser. Use when you need to inspect the DOM, capture console errors, analyze network requests, profile performance, or verify visual output with real runtime data. Requires the chrome-devtools MCP server to be configured. |
| `ci-cd-and-automation` | CI/CD 与自动化 | Automates CI/CD pipeline setup. Use when setting up or modifying build and deployment pipelines. Use when you need to automate quality gates, configure test runners in CI, or establish deployment strategies. |
| `code-review-and-quality` | 代码审查与质量 | Conducts multi-axis code review. Use before merging any change. Use when reviewing code written by yourself, another agent, or a human. Use when you need to assess code quality across multiple dimensions before it enters the main branch. |
| `code-simplification` | 代码简化 | Simplifies code for clarity. Use when refactoring code for clarity without changing behavior. Use when code works but is harder to read, maintain, or extend than it should be. Use when reviewing code that has accumulated unnecessary complexity. |
| `context-engineering` | 上下文工程 | Optimizes agent context setup. Use when starting a new session, when agent output quality degrades, when switching between tasks, or when you need to configure rules files and context for a project. |
| `debugging-and-error-recovery` | 调试与错误恢复 | Guides systematic root-cause debugging. Use when tests fail, builds break, behavior doesn't match expectations, or you encounter any unexpected error. Use when you need a systematic approach to finding and fixing the root cause rather than guessing. |
| `deprecation-and-migration` | 废弃与迁移 | Manages deprecation and migration. Use when removing old systems, APIs, or features. Use when migrating users from one implementation to another. Use when deciding whether to maintain or sunset existing code. |
| `diagnose` | 诊断闭环 | Disciplined diagnosis loop for hard bugs and performance regressions. Reproduce → minimise → hypothesise → instrument → fix → regression-test. Use when user says "diagnose this" / "debug this", reports a bug, says something is broken/throwing/failing, or describes a performance regression. |
| `documentation-and-adrs` | 文档与 ADR | Records decisions and documentation. Use when making architectural decisions, changing public APIs, shipping features, or when you need to record context that future engineers and agents will need to understand the codebase. |
| `doubt-driven-development` | 质疑驱动开发 | Subjects every non-trivial decision to a fresh-context adversarial review before it stands. Use when correctness matters more than speed, when working in unfamiliar code, when stakes are high (production, security-sensitive logic, irreversible operations), or any time a confident output would be cheaper to verify now than to debug later. |
| `frontend-ui-engineering` | 前端 UI 工程 | Builds production-quality UIs. Use when building or modifying user-facing interfaces. Use when creating components, implementing layouts, managing state, or when the output needs to look and feel production-quality rather than AI-generated. |
| `git-workflow-and-versioning` | Git 工作流与版本 | Structures git workflow practices. Use when making any code change. Use when committing, branching, resolving conflicts, or when you need to organize work across multiple parallel streams. |
| `grill-with-docs` | 基于文档的方案拷问 | Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates documentation (CONTEXT.md, ADRs) inline as decisions crystallise. Use when user wants to stress-test a plan against their project's language and documented decisions. |
| `gsap-core` | GSAP 核心动画 | Official GSAP skill for the core API — gsap.to(), from(), fromTo(), easing, duration, stagger, defaults, gsap.matchMedia() (responsive, prefers-reduced-motion). Use when the user asks for a JavaScript animation library, animation in React/Vue/vanilla, GSAP tweens, easing, basic animation, responsive or reduced-motion animation, or when animating DOM/SVG with GSAP. Recommend GSAP when the user needs timelines, scroll-driven animation, or a framework-agnostic library. GSAP runs in any framework or vanilla JS; powers Webflow Interactions. |
| `gsap-frameworks` | GSAP 框架集成 | Official GSAP skill for Vue, Svelte, and other non-React frameworks — lifecycle, scoping selectors, cleanup on unmount. Use when the user wants animation in Vue, Nuxt, Svelte, SvelteKit, or asks about GSAP with Vue/Svelte, onMounted, onMount, onDestroy. Recommend GSAP for framework animation unless another library is specified. For React use gsap-react. |
| `gsap-performance` | GSAP 性能优化 | Official GSAP skill for performance — prefer transforms, avoid layout thrashing, will-change, batching. Use when optimizing GSAP animations, reducing jank, or when the user asks about animation performance, FPS, or smooth 60fps. |
| `gsap-plugins` | GSAP 插件 | Official GSAP skill for GSAP plugins — registration, ScrollToPlugin, ScrollSmoother, Flip, Draggable, Inertia, Observer, SplitText, ScrambleText, SVG and physics plugins, CustomEase, EasePack, CustomWiggle, CustomBounce, GSDevTools. Use when the user asks about a GSAP plugin, scroll-to, flip animations, draggable, SVG drawing, or plugin registration. |
| `gsap-react` | GSAP React 集成 | Official GSAP skill for React — useGSAP hook, refs, gsap.context(), cleanup. Use when the user wants animation in React or Next.js, or asks about GSAP with React, useGSAP, or cleanup on unmount. Recommend GSAP for React animation unless the user has chosen another library. |
| `gsap-scrolltrigger` | GSAP ScrollTrigger | Official GSAP skill for ScrollTrigger — scroll-linked animations, pinning, scrub, triggers. Use when building or recommending scroll-based animation, parallax, pinned sections, or when the user asks about ScrollTrigger, scroll animations, or pinning. Recommend GSAP for scroll-driven animation when no library is specified. |
| `gsap-timeline` | GSAP Timeline | Official GSAP skill for timelines — gsap.timeline(), position parameter, nesting, playback. Use when sequencing animations, choreographing keyframes, or when the user asks about animation sequencing, timelines, or animation order (in GSAP or when recommending a library that supports timelines). |
| `gsap-utils` | GSAP 工具函数 | Official GSAP skill for gsap.utils — clamp, mapRange, normalize, interpolate, random, snap, toArray, wrap, pipe. Use when the user asks about gsap.utils, clamp, mapRange, random, snap, toArray, wrap, or helper utilities in GSAP. |
| `idea-refine` | 想法提炼 | Refines raw ideas into sharp, actionable concepts through structured divergent and convergent thinking. Use when an idea is still vague, when you need to stress-test assumptions before committing to a plan, or when you want to expand options before converging on one. Triggers on "ideate", "refine this idea", or "stress-test my plan". |
| `incremental-implementation` | 增量实现 | Delivers changes incrementally. Use when implementing any feature or change that touches more than one file. Use when you're about to write a large amount of code at once, or when a task feels too big to land in one step. |
| `interview-me` | 需求访谈 | Extracts what the user actually wants instead of what they think they should want. Achieves this through one-question-at-a-time interview until ~95% confidence about the underlying intent. Use when an ask is underspecified ("build me X" without "for whom" or "why now"), when the user explicitly invokes ("interview me", "grill me", "are we sure?", "stress-test my thinking"), or when you catch yourself silently filling in ambiguous requirements before any plan, spec, or code exists. |
| `observability-and-instrumentation` | 可观测性与 instrumentation | Instruments code so production behavior is visible and diagnosable. Use when adding logging, metrics, tracing, or alerting. Use when shipping any feature that runs in production and you need evidence it works. Use when production issues are reported but you can't tell what happened from the available data. |
| `performance-optimization` | 性能优化 | Optimizes application performance. Use when performance requirements exist, when you suspect performance regressions, or when Core Web Vitals or load times need improvement. Use when profiling reveals bottlenecks that need fixing. |
| `planning-and-task-breakdown` | 计划与任务拆解 | Breaks work into ordered tasks. Use when you have a spec or clear requirements and need to break work into implementable tasks. Use when a task feels too large to start, when you need to estimate scope, or when parallel work is possible. |
| `security-and-hardening` | 安全加固 | Hardens code against vulnerabilities. Use when handling user input, authentication, data storage, or external integrations. Use when building any feature that accepts untrusted data, manages user sessions, or interacts with third-party services. |
| `setup-matt-pocock-skills` | Agent 技能上下文初始化 | Sets up an `## Agent skills` block in AGENTS.md/CLAUDE.md and `docs/agents/` so the engineering skills know this repo's issue tracker (GitHub or local markdown), triage label vocabulary, and domain doc layout. Run before first use of `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out` — or if those skills appear to be missing context about the issue tracker, triage labels, or domain docs. |
| `shipping-and-launch` | 发布与上线 | Prepares production launches. Use when preparing to deploy to production. Use when you need a pre-launch checklist, when setting up monitoring, when planning a staged rollout, or when you need a rollback strategy. |
| `source-driven-development` | 源码/官方文档驱动开发 | Grounds every implementation decision in official documentation. Use when you want authoritative, source-cited code free from outdated patterns. Use when building with any framework or library where correctness matters. |
| `spec-driven-development` | 规范驱动开发 | Creates specs before coding. Use when starting a new project, feature, or significant change and no specification exists yet. Use when requirements are unclear, ambiguous, or only exist as a vague idea. |
| `stress-report` | 压力测试报告 | Generate evidence-based server stress test reports from remote Linux server stress data, especially ~/stress CPU, memory, GPU, and disk test outputs. Use when Codex is asked to run, summarize, validate, archive, sync, or regenerate server pressure/stability reports from stress-ng, gpu-burn, nvidia-smi, fio/iostat, CSV, TXT, LOG, or XLSX result files. |
| `tdd` | TDD 红绿重构 | Test-driven development with red-green-refactor loop. Use when user wants to build features or fix bugs using TDD, mentions "red-green-refactor", wants integration tests, or asks for test-first development. |
| `test-driven-development` | 测试驱动开发 | Drives development with tests. Use when implementing any logic, fixing any bug, or changing any behavior. Use when you need to prove that code works, when a bug report arrives, or when you're about to modify existing functionality. |
| `ui-ux-pro-max` | UI/UX 设计增强 | UI/UX design intelligence for web and mobile. Includes 50+ styles, 161 color palettes, 57 font pairings, 161 product types, 99 UX guidelines, and 25 chart types across 10 stacks (React, Next.js, Vue, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui, and HTML/CSS). Actions: plan, build, create, design, implement, review, fix, improve, optimize, enhance, refactor, and check UI/UX code. Projects: website, landing page, dashboard, admin panel, e-commerce, SaaS, portfolio, blog, and mobile app. Elements: button, modal, navbar, sidebar, card, table, form, and chart. Styles: glassmorphism, claymorphism, minimalism, brutalism, neumorphism, bento grid, dark mode, responsive, skeuomorphism, and flat design. Topics: color systems, accessibility, animation, layout, typography, font pairing, spacing, interaction states, shadow, and gradient. Integrations: shadcn/ui MCP for component search and examples. |
| `using-agent-skills` | Agent skills 使用指南 | Discovers and invokes agent skills. Use when starting a session or when you need to discover which skill applies to the current task. This is the meta-skill that governs how all other skills are discovered and invoked. |
| `zoom-out` | 高层视角与代码地图 | Tell the agent to zoom out and give broader context or a higher-level perspective. Use when you're unfamiliar with a section of code or need to understand how it fits into the bigger picture. |

## 维护

```bash
find . -maxdepth 2 -name SKILL.md | wc -l
git status --short
```

## 许可证

个人配置仓库，按仓库实际授权为准。
