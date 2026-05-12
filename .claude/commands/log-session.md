Analyze the current conversation and log the results into today's daily note. Proceed step by step:

## Step 1 — Find or create today's daily note

Today's date is $CURRENT_DATE. The daily note format is `DD.MM.YYYY.md`, located at:
`/home/jonas/Documents/obsidian_notes/Daily notes/MM.YYYY/DD.MM.YYYY.md`

Check whether the file exists. If not, create it with the following content:
```
#dailynote

## Todays todos: 
- [ ] 

## Progress:

## Problems:

## Questions:

## New Notes:

## Noteworthy:

## Meetings:

## To Do's:
```

Then read the existing file completely.

## Step 2 — Create new wiki notes

Look at all topics and concepts in the conversation. For each topic that is **new** (no existing wiki entry for it) and technically relevant:

1. Create a new note under the appropriate path in `/home/jonas/Documents/obsidian_notes/Wiki 2.0/`
2. The note should explain the topic in English, include source links (official docs, MDN, etc.), and set Obsidian wikilinks to related topics
3. Remember the filename for Step 3

Do **not** create a new note if one already exists for that topic.

## Step 3 — Update the daily note

Add content to the daily note — **always append, never overwrite** existing content:

### ## Progress:
Add concise bullet points about what was done in this session. Specific, brief, in past tense. Write in English. Examples:
- Created Svelte learning plan (phases 1–5, 16 exercises)
- Added wiki entries for Svelte Components and Reactivity
- Analyzed SAKE project structure

### ## New Notes:
Add a wikilink for each note created in Step 2:
- [[Note name]] — one-sentence description

### ## Problems:
If any questions, issues, or open points came up in the conversation: briefly summarize them and answer them directly below in 1–2 sentences. Format:
- **Problem/Question:** Short description
  → Short answer / solution

If there were no problems, do not modify this section.

## Important rules

- **Never delete or overwrite** existing content in the daily note
- Always append at the end of the respective section
- Do not add empty sections
- Calculate the date path correctly (month two digits, year four digits)
- If no daily note exists for today, create it first, then fill it in
- All wiki notes and daily note entries must be written in **English**
- At the end: briefly report what was logged
