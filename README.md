## Resume Helper

Python tools plus a Cursor agent workflow for tailoring a master LaTeX resume
to specific job posting PDFs, while enforcing strict one-page and layout rules.

Core pieces:

- `resume_helper/`: Python package with parsing, selection, generation, and analysis logic.
- `data/master/`: Master resume sources (`master_resume.tex`, `master_resume.pdf`).
- `data/jobs/<job-id>/`: One folder per application, containing `job_posting.pdf` and generated artifacts.
- `data/knowledge/knowledge_base.json`: Long-term JSON knowledge base built from your resume plus Q&A.

You will typically interact with this via the Cursor agent configured in
`.cursor/rules/resume-helper.mdc` (to be added), by placing a job posting PDF in
`data/jobs/<job-id>/` and asking the agent to tailor a resume for that job.

# Resume-Helper
Quick repository I made to help me tailor my resumes to the job I am aplying for.
