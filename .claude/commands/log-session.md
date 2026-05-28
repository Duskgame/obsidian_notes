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

Look at all topics and concepts in the conversation. For each topic that is **new** (no existing wiki entry for it) and technically relevant, run the **write-note** skill for that topic. The write-note skill handles:
- Checking whether a note already exists (and updating it if so)
- Choosing the correct directory and creating subdirectories when needed
- Writing the note in the correct format with sources, code examples, and wikilinks
- Adding backlinks from related existing notes

Remember the final path and filename of each created or updated note for Step 3.

## Step 3 — Update the daily note

Add content to the daily note — **always append, never overwrite** existing content:

### ## Progress:
Add bullet points about actual work done in this session — coding, decisions made, tasks completed, things learned that matter for ongoing projects. Specific, brief, in past tense. Write in English.
Link notes that were **updated** (existing notes that received new content) inline in the relevant bullet.
Examples:
- Fixed sync conflict resolution in Kwizz backend
- Decided to use FIFO queue for payment flow
- [[Existing Note]] — added section on X

Do NOT list newly created notes here — those go under New Notes.

### ## New Notes:
Add a wikilink for each note **newly created** in Step 2:
- [[Note name]] — one-sentence description

### ## Questions:
If any open questions came up in the conversation that the user or their organisation should follow up on — topics to research further, decisions not yet made, things to verify — add them here. Format:
- **Question:** Short description of what needs to be followed up

Only add genuine open questions, not things that were already answered in the conversation.
If there were no open questions, do not modify this section.

### ## Problems:
If any technical issues, blockers, or unresolved errors came up in the conversation: briefly summarize them and answer them directly below in 1–2 sentences. Format:
- **Problem:** Short description
  → Short answer / solution or workaround

If there were no problems, do not modify this section.

## Important rules

- **Never delete or overwrite** existing content in the daily note
- Always append at the end of the respective section
- Do not add empty sections
- Calculate the date path correctly (month two digits, year four digits)
- If no daily note exists for today, create it first, then fill it in
- All wiki notes and daily note entries must be written in **English**
- At the end: briefly report what was logged
