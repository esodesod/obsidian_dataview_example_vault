---
description: %% What does the query do? %%
topics:
  - 
---
#dv/


> [!hint] Contributed by PutYourNameOrSourceHere

# Basic Task Queries

> [!todo] Create a new Query Note
> - [x] Use this template
> - [ ] Write a short description of the query in the frontmatter
> - [ ] Write the most basic version of the query possible. Keeping it simple and reduce it to the necessary makes it easier for others to understand the important parts of a query
> - [ ] If applicable, add one or multiple variants of the query and explain what they are doing - this could i.e. be an enhancement that makes the result better readable/even more useful
> - [ ] Add appropriate tags to the page 
> 	_Hint: Adding queries often? Consider using the [[Templater Tag Auto Generation]]_
> 	- [ ] If a DQL query: used query type (LIST, TABLE etc), data commands (FROM, WHERE, FLATTEN etc) and used [functions](https://blacksmithgu.github.io/obsidian-dataview/query/functions/) - always with #dv in front, i.e. #dv/list 
> 	- [ ] If a dataviewjs query: #dv/dataviewjs and every [dataviewjs](https://blacksmithgu.github.io/obsidian-dataview/api/code-reference/) or [data array](https://blacksmithgu.github.io/obsidian-dataview/api/data-array/) function as a #dvjs tag, i.e. #dvjs/current 
> - [ ] Add topics to the frontmatter that helps the query at the end of the page to list similar queries. Head over to the [[Topic Overview]] to see what's available and be welcome to introduce new topics where appropriate!
> - [ ] Add your name in the contribution callout at the top - or remove the contribution callout, however you like it better! Mind, though: Before contributing queries from others, always ask for their permission.
> - [ ] Delete this callout :) 

## Basic 

> [!hint] Relationship between Tasks and Pages
> The `TASK` query type is special, because unlike all other query types (`LIST`, `TABLE`, `CALENDAR`) `TASK` does not operate on **page level** but on **task level**. If a page contains three tasks, you'll get back **three** results instead of one, like you'd get with any other query type. This allows us to filter for task specific properties, like included tags, meta data or text. Be aware though, that **every task inherits all properties from the page it's in**. Read more about [how task queries behave](https://blacksmithgu.github.io/obsidian-dataview/query/queries/#task-queries) and [what for information you have available](https://blacksmithgu.github.io/obsidian-dataview/data-annotation/#tasks).

**List tasks  from a folder**
```dataview
TASK
FROM "10 Example Data/assignments"
```

**List tasks from a tag**
```dataview
TASK
FROM #next
```

**Combine multiple tags**
```dataview
TASK
FROM #later OR #clientA
```

**Combine multiple folders**
```dataview
TASK
FROM "10 Example Data/assignments" OR "10 Example Data/games"
```

**Combine tags and folders**
```dataview
TASK
FROM "10 Example Data/assignments" AND #later  
```

**List all tasks, everywhere**

> [!attention] Add `dataview` to code block
> The output of this is pretty long. If you want to see it, add `dataview` to the code block - like on the examples above!

```
TASK
```

## Variants

### List only open tasks

```dataview
TASK
FROM "10 Example Data/assignments"
WHERE !completed
```

### Group tasks by file

```dataview
TASK
FROM "10 Example Data/assignments"
GROUP BY file.link
```

### Show tasks that have a certain tag

```dataview
TASK
FROM "10 Example Data/assignments"
WHERE contains(tags, "#later")
```

### Show tasks with a due date (or any other meta data field)

```dataview
TASK
FROM "10 Example Data/assignments"
WHERE duedate
```

### Sort task after completion date

> [!attention] Only possible with designated meta data
> A task does not know its completion date out of the box. In order to query for it, you need to add it to the task as an inline meta data field. Dataview provides an automatism for this: In the dataview options, at the very bottom, activate "Automatic Task Completion Date" to automatically append a completion date when checking a task.
> ![[Pasted image 20220906152304.png|500]]
> ⚠ **Attention!** Dataview can only add this information **if you check a task inside a dataview output** - meaning inside a TASK Query!

```dataview
TASK
FROM "10 Example Data/assignments"
WHERE completed
SORT completion
```

### List only open tasks

---
%% === end of query page === %%
> [!help]- Similar Queries
> Maybe these queries are of interest for you, too:
> ```dataview
> LIST
> FROM "20 Dataview Queries"
> FLATTEN topics as flattenedTopics
> WHERE contains(this.topics, flattenedTopics)
> AND file.name != this.file.name
> ```

```dataviewjs
const inlinksFromUseCases = dv.current().file.inlinks.filter(link => link.path.contains("33 Use Cases"));

const header = `> [!info] Part of Use Cases`;

if (inlinksFromUseCases.length > 1) {
	const list = inlinksFromUseCases.array().reduce((acc, curr) => `${acc}</br> - ${curr}`,"")

	dv.span(`${header}
    > This query is part of following use cases:
    > ${list}
    > 
	`)
} else if (inlinksFromUseCases.length === 1) {
	dv.span(`${header}
    > This query is part of use case ${inlinksFromUseCases[0]}.
	`)
}
```