# Resume Helper

A **Cursor AI agent workflow** for tailoring a master LaTeX resume to specific job postings. This tool uses Cursor's AI capabilities to analyze job descriptions, extract relevant experiences from your master resume, and generate tailored one-page resumes—all without running any external Python scripts.

## Overview

Resume Helper is a pure Cursor wrapper that leverages the AI agent configured in `.cursor/rules/resume-helper.mdc` to:
- Extract and structure information from job posting PDFs
- Match job requirements to your experiences
- Generate tailored LaTeX resumes that fit on exactly one page
- Maintain a long-term memory of your background to avoid repetitive questions

## Prerequisites

- [Cursor](https://cursor.sh/) editor installed
- Your master resume in LaTeX format
- **LaTeX distribution** (for compiling tailored resumes to PDF) — optional but recommended; the agent can compile and inspect the PDF to validate layout when a compiler is available (see below)

## LaTeX: dependency and installation

Resume Helper produces `.tex` files. To get PDFs (and to let the agent validate the output), you need a LaTeX distribution that provides `pdflatex`. On **Windows**, use one of the following.

### Option 1: MiKTeX (recommended on Windows)

1. **Download**  
   Go to [miktex.org/download](https://miktex.org/download) and download the **Windows** installer (e.g. “Basic MiKTeX Installer”).

2. **Install**  
   - Run the installer.  
   - Choose **“Install for: Just for me”** (or “All users” if you prefer).  
   - When asked about missing packages, choose **“Install missing packages on-the-fly: Yes”** so MiKTeX can download packages when you compile.  
   - Finish the installation.

3. **Verify**  
   Close and reopen PowerShell or Command Prompt, then run:
   ```powershell
   pdflatex --version
   ```
   If you see a version line, you’re set. The first time you compile a resume, MiKTeX may download a few packages (e.g. `fontawesome5`, `glyphtounicode`).

### Option 2: TeX Live

1. **Download**  
   Go to [tug.org/texlive](https://tug.org/texlive/) → **acquire** → **install-tl-windows** and download the Windows net installer (`install-tl-windows.exe`).

2. **Install**  
   Run the installer and choose **“Full installation”** (or “medium” if you want a smaller install). The download can take a while.

3. **Verify**  
   TeX Live adds its `bin` folder to your PATH (e.g. `C:\texlive\2024\bin\windows`). Open a **new** terminal and run:
   ```powershell
   pdflatex --version
   ```

### If `pdflatex` is not found

- **MiKTeX:** Ensure its `bin` folder is on your PATH (e.g. `C:\Users\<You>\AppData\Local\Programs\MiKTeX\miktex\bin\x64`).  
- Restart Cursor (and any terminal) after installing so it picks up the new PATH.

### Compiling a tailored resume manually

From the job folder:

```powershell
cd data\jobs\<job-id>
pdflatex -interaction=nonstopmode tailored_resume.tex
```

Run it twice if the resume uses references. The agent will do this automatically when a LaTeX toolchain is available and will inspect the PDF to check layout rules.

## Setup

1. **Clone or download this repository**

2. **Set up your master resume**
   - Place your master LaTeX resume at `data/master/main.tex`
   - Optionally include a compiled PDF at `data/master/main.pdf` for reference
   - The master resume should have:
     - A **fixed header** (name, contact info)
     - A **mutable middle section** (experiences, skills)
     - A **fixed footer** (education)

3. **Initialize your conversation memory** (optional but recommended)
   - Copy `data/master/conversation_memory.example.json` to `data/master/conversation_memory.json`
   - Fill in your background information to help the agent avoid asking repeat questions
   - This file stores Q&A history and skill-to-experience mappings

## Usage

### Step 1: Prepare a Job Posting

For each job application:

1. Create a folder under `data/jobs/` with a descriptive name (e.g., `data/jobs/clinia-frontend-2026/`)
2. Save the job posting PDF as `job_posting.pdf` in that folder

### Step 2: Ask Cursor to Tailor Your Resume

In Cursor, open a chat and say something like:

```
Tailor my resume for the job posting at data/jobs/clinia-frontend-2026/job_posting.pdf
```

Or if you have the job folder open:

```
Tailor my resume for this job posting
```

### Step 3: Answer Questions (if needed)

The agent may ask 2-3 targeted questions about your background if:
- A skill is mentioned in the job posting but not clearly mapped in your resume
- Additional context would help tailor the resume better

These answers are saved to `conversation_memory.json` so you won't be asked again for future jobs.

### Step 4: Review and Compile

The agent will generate:
- `job_posting_extracted.json` — structured data extracted from the PDF
- `tailored_resume.tex` — your customized resume in LaTeX

If a LaTeX distribution is installed (see [LaTeX: dependency and installation](#latex-dependency-and-installation)), the agent can compile the `.tex` file to PDF and inspect the output to validate layout. Otherwise, you can compile manually or use the LaTeX as-is.

## File Structure

```
Resume-Helper/
├── .cursor/
│   └── rules/
│       └── resume-helper.mdc    # Cursor agent configuration
├── data/
│   ├── master/
│   │   ├── main.tex                          # Your master resume (required)
│   │   ├── main.pdf                          # Compiled master resume (optional)
│   │   ├── conversation_memory.json          # Your background Q&A database
│   │   └── conversation_memory.example.json  # Template for conversation memory
│   └── jobs/
│       ├── example/
│       │   └── job_posting_extracted.example.json  # Template for job extraction
│       └── <job-id>/                          # One folder per application
│           ├── job_posting.pdf                # Job description PDF
│           ├── job_posting_extracted.json     # Extracted job data (generated)
│           └── tailored_resume.tex            # Tailored resume (generated)
└── README.md
```

## How It Works

The Cursor agent follows this workflow:

1. **Reads the job posting PDF** and extracts structured information
2. **Analyzes your master resume** to identify relevant experiences
3. **Checks conversation memory** to avoid asking repeat questions
4. **Selects the most relevant experiences** (typically 3-5) that match the job
5. **Generates tailored LaTeX** with:
   - Your fixed header and footer (unchanged)
   - Selected experiences with relevant bullets
   - Key skills from the job posting **bolded** for ATS scanning
6. **Ensures one-page layout** by adjusting content and bullet lengths

## Important Notes

- **Personal data is ignored**: `.gitignore` is configured to ignore `.tex`, `.pdf`, and `.json` files to protect your personal information
- **Example files are tracked**: Template files with `.example.json` extension are included for reference
- **No Python required**: This is a pure Cursor workflow—no scripts or dependencies to install
- **One-page constraint**: The agent strictly enforces a one-page limit and will adjust content accordingly

## Tips

- **Build your conversation memory over time**: The more you use Resume Helper, the fewer questions you'll be asked as the agent learns your background
- **Review the tailored resume**: Always review the generated resume and make manual adjustments if needed
- **Keep your master resume updated**: Add new experiences to `main.tex` so they're available for future tailoring

## Example Workflow

```bash
# 1. Create job folder
mkdir -p data/jobs/clinia-frontend-2026

# 2. Add job posting PDF
# (Save PDF as data/jobs/clinia-frontend-2026/job_posting.pdf)

# 3. In Cursor chat:
"Tailor my resume for data/jobs/clinia-frontend-2026/"

# 4. Answer any questions the agent asks

# 5. Review tailored_resume.tex and compile to PDF
```

## License

MIT License - feel free to adapt this workflow for your own use!
