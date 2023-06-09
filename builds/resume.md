---
title: Resume 
tagline: Building a system to version my resume and generate a pdf of it
---

# Resume
gh: [https://github.com/lukew3/resume](https://github.com/lukew3/resume)

## The Idea
The story of this project starts when I was looking for a good template for my resume. My first resumes were created in Microsoft Word/ Google Docs, but I soon realized that these resumes were visually unappealing and I wanted my resume to look like the cool latex ones that I had seen other people post online.

While searching for latex templates and latex resume builders, I found the website [https://resumake.io](https://resumake.io). The site is well-designed and a pleasure to use, and the fact that it's open-source made me like it even more. I loved the first template on their site. It was compact but neat enough that sections and items seperated themselves visually. I used the site for a couple of days and iteratively made improvements to my resume. I thought to myself that it would be nice to have a record of how my resume has changed over time. But pdfs are large and uploading a new pdf on every commit results in very large and hard to understand git commits. I noticed that resumake had an option to download the resume as LaTeX and an option to download as JSON. LaTeX would be a good option, but what about JSON?

I looked into the schema of the JSON file and tried searching to see if there was a standard schema for JSON resumes and sure enough found [https://jsonresume.org](https://jsonresume.org). I dug into the ecosystem of projects working with jsonresume, and thought that I saw a gap. There was not a project that simply took the data of a jsonresume file and inserted it into a latex document and created a pdf. Resumake did this, but requires users to use their web ui or drop in json and click some buttons to make it and generate a new pdf. I saw a perfect opportunity to use Github Actions.

## Implementation
Github Actions allows open-source repositories to run code on Github servers on certain Github events. In my case, when a user publishes a new version of their json resume, I wanted to tell Github Actions to fill a LaTeX template with the contents of my jsonresume and create a pdf of it. Github Actions work as a series of steps that the runner takes from the moment an action is triggered to when it is finished. Gihub has a marketplace for actions where I found an action that could [build tex to pdf](https://github.com/xu-cheng/latex-action) and other actions for uploading files to a release, but I still needed an action that would populate a LaTeX file with the contents of a jsonresume. 

I decided that the erb template style popularized by ruby on rails would be best suited for filling a LaTeX template because erb delimiters look most out of place in LaTeX code. Maybe I could have used another templating engine but changed the delimiters, but this was what I was familiar with and the Ruby code was a clean 4 lines. I figured that there might be a chance that somebody else would want to use this in their Github Actions, so I created a [seperate repo for this function](https://github.com/lukew3/json-fill-erb-action/tree/main) and published it.

When making changes to the json directly, I realised that it was easy to make mistakes and not follow the jsonresume schema. I decided to make another action for doing this. Luckily jsonresume [provides their own javascript package for validating the schema of a jsonresume](https://github.com/jsonresume/resume-schema) so all I had to do was create a wrapper workflow that ran their validation function on the json provided to a Github Action. You can see how I did this or use it yourself at [lukew3/valiate-json-resume-action](https://github.com/lukew3/validate-json-resume-action).

Plugging this all together made the experience of updating and keeping track of my resume versions extremely easy. Another thing that I am really happy about is that Github allows one link that always directs the follower to the latest download of the pdf, found at [https://github.com/lukew3/resume/releases/latest/download/lukew3_resume.pdf](https://github.com/lukew3/resume/releases/latest/download/lukew3_resume.pdf).

## Conclusion
I love how this project works, and always share it with people who are interested in latex resumes because I think that it is a delight to use.
