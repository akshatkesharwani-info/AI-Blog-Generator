# AI Blog Generator — Give a Topic, Get a Full Blog Post, SEO Text and a Header Image

A Colab notebook that turns one topic into a complete markdown blog post with a meta description, tags and a header image. If the free image service does not answer, it makes a title banner so the notebook always finishes.

## Problem

Writing a blog from zero means an outline, a draft, SEO text and a header picture. That is a lot of steps for one post.

## Approach

1. Ask for a topic, tone (friendly, professional, funny) and word count
2. Make an outline with an LLM on Groq (`openai/gpt-oss-120b`)
3. Write the full blog in markdown from that outline (rules in the prompt: simple English, no made-up statistics or quotes)
4. If a long blog (300+ words) comes out under 70% of the length asked, expand it once
5. Ask for a meta description (under 150 characters), 5 tags and a one-scene image prompt
6. Ask a free image service for 3 header options at the same time, waiting at most 75 seconds. If nothing comes back, draw a title banner with Pillow
7. Save `blog.md` and the header image

## Tech Stack

Python, Groq API (OpenAI-compatible client), requests, Pillow, matplotlib, Google Colab

## Results

One real run, topic "gaming", tone friendly, default length (600 words):

| Item | Result |
|---|---|
| Blog length | 725 words (asked for about 600, so 21% over) |
| Structure | Title, intro, 6 sections, conclusion |
| Meta description | 101 characters |
| Tags | 5 |
| Header image | Title banner (the free image service gave no image within 75 seconds) |
| Run | No errors, no hang |

What the earlier runs showed, and what changed because of it:

- **Length is approximate.** Asking for 100 words gave 158.
- **The first image was bad.** The image prompt asked for a "timeline collage", and the service returned a 6-panel collage of neon boxes with its own watermark and a garbled word. The prompt now asks for one single scene with no text.
- **The banner title was tiny** (about 5 pixels) because the font file was not found. It now uses the font that ships with matplotlib (about 50 pixels).
- **The image step could hang** because it asked for three images one after another with no time limit. It now asks for all three at once and gives up after 75 seconds.

## How to Run

1. Open `03_AI_Blog_Generator.ipynb` in Google Colab
2. Get a free API key from [console.groq.com](https://console.groq.com)
3. Click Runtime → Run all, paste the key when asked
4. Type a topic. Press Enter for tone and for 600 words
5. Press Enter for the AI image (or type 2 for the banner). If images appear, run Step 8b and pick one

## Limitations

- The 3-option image picker is built and tested with simulated responses, but in my final live run the service returned nothing, so I have not seen it return images with the new prompt
- The free image service can add its own watermark, and its terms apply. Check them before using images commercially
- The outline step sometimes counts the title as item 1, so the first section can repeat the title (it did in my run)
- Blog length is approximate
- The blog is AI written. Read it and check facts before posting
- Tested on one topic

## Next Steps

- Fix the outline step so the title is not repeated as a section
- Use an image model that runs in the notebook (no watermark, no waiting on a free service)
- Make many blogs from a list of topics
- Add a fact-check step

## Author

**Akshat Kesharwani** | [GitHub](https://github.com/akshatkesharwani-info) | [Portfolio](https://akshatkesharwani-info.github.io/)
