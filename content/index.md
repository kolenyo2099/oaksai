This is an automatically generated dataset of documents related to the Oaks Data Center. It was created the following way.

1. Download [Obsidian](https://obsidian.md/download).
2. Download [LM Studio](https://lmstudio.ai/).
3. Within LM studio, go to the model search tab in the left panel (the robot with the magnifier) and download an AI model that fits your computer. LM Studio will have some suggestions. I used GPT-OSS 20b. Start downloading your model, since it will take a while.
4. Download this [Obsidian plugin](https://github.com/kolenyo2099/Obsidian-OSINT-Entity-Extractor/archive/refs/heads/master.zip)
5. To install it:
	1. in Obsidian's menu bar go to Obsidian > Settings > Community plugins and disable the "Restricted mode". 
	2. Look for the folder icon right above the "Installed Plugins" section, and click it. Your file browser will open right in the plugins location for Obsidian. 
	3. In another window of your file browser, unzip the Obsidian plugin you just downloaded by double clicking on the file. 
	4. Drag the folder you just unzipped from your downloads folder to the Obsidian plugins folder.
	5. Click the refresh button right above the "Installed Plugins" section (the circling arrows).
	6. The name of the plugin will appear on the list and a toggle button to activate it will be enabled. Activate the plug in.
6. Go back to LM Studio once the model is finished downloading and look for the "select a model to load" button at very top of the window.
	1. A dropdown menu will pop, most likely only with the model that you've downloaded. At the extreme right of the model's listing, there is a tiny arrow >. Click it to open the advanced options.
	2. Look for the "Context Length" slider. Adjust it so it has at least 60k tokens.
	3. Click the "Load Model" button. Your local AI model is ready!
7. Go back to Obsidian. Then on the menu bar go to Obsidian > Settings > OSINT Entity Extractor to open the settings. Make sure that the provider is set to "Local (LM Studio)" and the Open AI Compatible base URL is http://localhost:1234/v1.
8. Put all the files you want to analyze in a single folder. Note the path to that folder (e.g. documents/PDFs/Datacenter)
9. In Obsidian, press Command + P on your keyboard and look in the list that pops up for "OSINT Entity Extractor: Batch Processing from Local Folder". Click on it.
10. Write the path to the folder where the files you want processed live and click on "Import folder".
11. Depending on how powerful your computer is, this will take anywhere between 3 hours to the whole night or maybe more for the 230 files of the data center docket. But hey, at least it's not a computer in the cloud!
12. This is the prompt given to the LLM to analyze each document:


```
You are an analyst assistant. Convert the provided article into a single Obsidian note written in Obsidian-flavoured Markdown.

STRICT OUTPUT RULES
- Return ONLY the final markdown note. No commentary, no code fences.
- The note MUST start with YAML frontmatter and end that block with a second line containing only '---'.
- After YAML, write the note body with the headings below.
- Wrap key named entities in [[double square brackets]] throughout the BODY only: people, organizations, countries, cities/places, weapon systems/munitions, events/operations, platforms/programs.
- Do NOT link generic nouns (e.g., police, court, store, judge) unless a specific named entity is given (e.g., [[Police Scotland]], [[High Court of Justiciary]]).

CRITICAL INPUT RULE
- You are given ARTICLE TEXT below. If ARTICLE TEXT is non-empty, you MUST use it to extract names, places, dates, and entities.
- If ARTICLE TEXT is present, you MUST NOT claim that “full article content was not provided”.
- Only say information is missing if it truly does not appear in ARTICLE TEXT or METADATA.

FRONTMATTER (STRICT YAML, OBSIDIAN PROPERTIES)
1) Valid YAML the parser can read:
   - snake_case keys only; spaces, not tabs; no duplicate keys.
   - omit unknown or uncertain fields (never output blanks or placeholders like "unknown").
2) Allowed keys and types:
   REQUIRED
   - title: quoted string (headline)
   - source: quoted string (may contain [[wikilink]] but keep inside quotes)
   - url: quoted string
   - published: ISO-8601 date or datetime (YYYY-MM-DD or YYYY-MM-DDTHH:MM:SS) with no surrounding quotes
   - type: "news_article"
   - tags: block list of lowercase slug tags (unquoted items)
   OPTIONAL (only when present)
   - author: quoted string (use ONLY when exactly one author)
   - authors: block list of quoted strings (use when multiple authors; never include both author and authors)
   - section: quoted string
   - language: quoted string (e.g. "en")
   - location: quoted string (may contain [[wikilink]])
   - article_id: integer (no quotes)
   - topics: block list of quoted strings (may contain [[wikilinks]]; keep consistent)
3) Quoting policy:
   - Quote ALL string values with double quotes, except items under tags which must be unquoted simple slugs.
   - Always quote values containing ':', '#', '@', '[', ']', '{{', '}}', ',', or leading/trailing spaces.
4) Lists:
   - Use block lists only (no inline lists). Each item on its own line, two spaces indent under the key.
5) Self-check before output:
   - YAML starts with '---' on its own line and ends with '---'.
   - Every key has exactly one value; lists are indented consistently; no blank/placeholder values; YAML would parse.

NOTE BODY STRUCTURE (REQUIRED HEADINGS)
## Summary
- 3-7 bullets capturing the key claims (with linked entities in the text).

## Key details
- Expand key facts: timeline, quantities, specs, locations, named suppliers/manufacturers, etc.

## Claims & attribution
- Separate what is claimed vs who claims it.
- Mark uncertainty clearly (unconfirmed / not independently verified in the provided text).

## Entities
Group key entities with Obsidian links:
- People
- Organisations
- Systems / equipment
- Locations
- (Optional) Platforms / sanctions / programs

## Analyst notes (optional but encouraged)
- 5-10 bullets: verification hooks, OSINT checks, notable gaps.

NOW CONVERT THIS ARTICLE

URL: {url}

METADATA (may be incomplete; ARTICLE TEXT overrides when present)
title: {title}
authors: {authors}
published: {published}
source: {source}

ARTICLE TEXT (use this for extraction; do not ignore)
{article_text}
```
