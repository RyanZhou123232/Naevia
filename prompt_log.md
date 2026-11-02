# Naevia — Phase 1 Prompt Log

## Scope and provenance

Reconstructed on November 1, 2026 from the original Codex chat **概述项目用途** (October 28, 2026). The prompts below are copied from actual user messages, not invented prompts or AI paraphrases. They document system design, asset requirements, and the publication/consolidation decisions behind the art/spec handoff. This is a late documentation supplement, not a file claimed to have been committed during the original class period.

Tools used: Codex desktop for specification/system-design and GitHub handoff assistance; GitHub tools for publishing documents; imagegen for artwork, as recorded in SPEC section 3.3. Exact historical Codex model identifiers and the image-generation model version were not recorded in the recovered source. They are unknown and are not guessed here. The separate visual agent's original generation prompts were not recovered in this log; the art references, export manifest, and validation report remain the available provenance. This is a recovered important-prompt log, not a complete transcript of every tool invocation.

The private/local game implementation developed on October 29 is outside this Phase 1 GitHub submission. No local application code, credentials, or that later implementation's prompts are included.

## Original user prompts — October 28

### 1 — Original turn 01a1837b-6cb4-7530-aa2b-a7d7013c6457

```text
我觉得现在需要做的是我们一起做一下系统设计。我会让另外一个agent同时做全套前端asset设计
```


### 2 — Original turn 01a1837e-b90c-7030-b8fc-fb926c3b619a

```text
现在本地就行。我觉得这样。你把我这个file整理以下。所有的文件放进doc里面。然后这个demo可以在本地就行。我们来设计一下其他的逻辑
```


### 3 — Original turn 01a18384-6209-7432-8136-ac245b6df58d

```text
你现在给我high level的过一下整个系统设计。从前端用什么host。以前我的expertise只有网页前端。但你给我讲一下现在这个本地前端用什么host，后端怎么处理，然后什么时候执行上面操作，跟数据库怎么交互，数据存在哪里，数据库是什么类型。例如我到时候肯定有一系列的视觉asset，后端告诉我这个像素应该是钻石，那前端这里就要显示钻石像素，这个asset数据库和memory数据库肯定是不一样的，你觉得这些要砸设计
```


### 4 — Original turn 01a1838d-8a6d-7103-8b6c-18de506b146e

```text
ok，那你现在需要我的视觉agent做什么。你需要他给你生成的图片或者啥符合什么格式。你给我具体你要的需求，我让她做，demo1你需要什么
```


### 5 — Original turn 01a1839b-d4a6-75d0-a0bb-03a9bdfd49fb

```text
我觉得这样，你可能要改一下sepc。整体demo1 的要求是不用变。你改一下spec因为我现在以及做好了视觉嘛。让下一个人，拿到我现在做好的视觉。搭建demo1.然后德莫的要求还是不变的哈。你看看现在要怎么做
```

```text
你别
```

```text
还没有生成完
```

```text
还在继续生成呢
```


### 6 — Original turn 01a183a1-cd11-7f93-a5a6-b856d30195ba

```text
算了你不用重写写spec了。我觉得你可以做的事情是，我们不是有新的spec where我们有明确的验收目标嘛，就是技术路径验收目标啥的都写进去的一个版本。我觉得你需要1）把这个新版本的spec放到github。2）等我视觉做完了，把视觉push上去。别人可以看着spec就知道接着视觉要往哪做了。如果现在没有一个你觉得完整的spec，你可以重写spec。如果你觉得现在的spec都完整，验收和清晰的技术路径都有了，就不用写新spec了。你看看你觉得现在要怎么做
```


### 7 — Original turn 01a183a5-e025-7790-a41d-d3e7f0c56922

```text
我之前没有说清楚。这个项目的handoff我只能在github上方我的spec，readme和doc都不能放上去prd也不行。所以我需要你很清晰的说我需要他拿我放上去的视觉，然后他要用什么技术路径做成什么，然后demo我的验收需要看到那些东西。这个要求你看看现在的spec满不满足（应该是不满足的因为你验收报告是单独的doc，但我只能放一个spec）。然后你可能要重写spec然后重写上传然后把其他doc删掉
```


### 8 — Original turn 01a183b8-e3f4-7ae3-904c-2fb0d799ec16

```text
ok做好了你看看
```

## What the record explains

The original owner instructed consolidation into SPEC.md and removal of README/other documents. Commit `809b464` records that consolidation. This explains the missing README found during the November 1 course checklist review; the course still requires separate README.md and prompt_log.md files. The current owner then explicitly instructed adding only those two documents and keeping Repo 1 as art/spec, without uploading the local game code.

## November 1 documentation request — verbatim

```text
repo1就是那些art和spec，但你要补readme（或者上传readme）和prompt log
```

Only README.md and prompt_log.md are added by this supplement. Existing SPEC.md, art files, and historical commits are preserved. The README reports gameplay requirements as not implemented or only partially supported by art, rather than presenting the later local game as submitted work.

