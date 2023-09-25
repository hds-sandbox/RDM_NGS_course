﻿---
title: DTU workshop 2023
summary: This is a summarized workshop focused on practical lessons of RDM for NGS data
tags:
    - Practical lessons
    - Exercises
    - DTU workshop
---

# DTU workshop October 2023

**Last updated:** *{{ git_revision_date_localized }}*

!!! note "Section Overview"

    &#128368; **Time Estimation:** 75 minutes  

    &#128172; **Learning Objectives:**    
        
    1. Organize and structure your data and data analysis with cookiecutter templates
    2. Establish metadata fields and collect metadata when creating a cookiecutter folder
    3. Establish naming conventions for your data
    4. Make a catalog of your data
    5. Create GitHub repositories of your data analysis and display them as GitHub Pages
    6. Archive GitHub repositories on Zenodo

This is a practical version of the full RDM on NGS data workshop. The main key points of the exercises shown here is to help you organize and structure your NGS datasets and your data analyses. We will see how to keep track of your experiments metadata and how to safely version control and archive your data analyses using GitHub repositories and Zenodo. We hope that through these practical exercises and step-by-step guidance, you'll gain valuable skills in efficiently managing and sharing your research data, enhancing the reproducibility and impact of your work.

!!! warning "Requirements"

    In order to follow this tutorial you will need:
    
    - A [GitHub account](https://github.com/)
    - A [Zenodo account](https://zenodo.org/)
    - [Python](https://www.python.org/) and [pip](https://pip.pypa.io/en/stable/installation/) installed
    - MkDocs and MkDocs material theme (can be installed through pip)

    In addition, you should install the following MkDocs extensions

    ```bash
    pip install mkdocs
    pip install mkdocs-material
    pip install mkdocs-minify-plugin
    pip install mkdocs-git-revision-date-localized-plugin 
    pip install mkdocs-jupyter
    pip install mkdocs-table-reader-plugin
    ```

## 1. Organize and structure your NGS data and data analysis

Applying a consistent file structure and naming conventions to your files will help you to efficiently manage your data. We will divide your NGS data and data analyses into two different types of folders:

1. **Assay folders**: These folders contain the **raw and processed NGS datasets**, as well as the **pipeline/workflow** used to generate the processed data, provenance of the raw data and quality control reports of the data. This data should be **locked and read-only** to prevent unwanted modifications.
2. **Project folders**: These folders contain **all the necessary files for a specific research project**. A project may use several assays or results from other projects. The assay data should not be copied or duplicated, but linked from the original source.

Projects and Assays are separated from each other because a project may use one or more assays to answer a scientific question, and assays may be reused several times in different projects. This could be, for example, all the data analysis related to a publication (a RNAseq and a ChIPseq experiment), or a comparison between a previous ATACseq experiment (which was used for a older project) with a new laboratory protocol.

You could also create **Genomic resources folders** such things such as genome references (fasta files) and annotations (gtf files) for different species, as well as indexes for different alignment algorithms. If you want to know more, feel free to check the relevant [full lesson](./06_file_structure.md#genomic-resources-folder)

This will help you to keep your data tidied up, specially if you are working on a big lab where assays may be used for different purposes and different people!

### Assay folder

For each NGS experiment there should be an `Assay` folder that will contain all experimental datasets, that is, an `Assay` (raw files and pipeline processed files). Raw files should not be modified at all, but you should probably lock modifications to the final results once you are done with preprocessing the data. This will help you prevent unwanted modifications to the data. Each `Assay` subfolder should be named in a way that it is unique, easily readable, distinguishable and understood at a glance. For example, you could name an NGS assay using an acronym for the type of NGS assay (RNAseq, ChIPseq, ATACseq), a keyword that represents a unique descriptive element of that assay, and the date. Like this:

```
<Assay-ID>_<keyword>_YYYYMMDD
```

For example `CHIP_Oct4_20230101` is a ChIPseq assay made on 1st January 2023 with the keyword Oct4, so it is easily identifiable by eye. Next, let's take a look at a possible folder structure and what kind of files you can find there.

```bash
CHIP_Oct4_20230101/
├── README.md
├── metadata.yml
├── pipeline.md
├── processed
└── raw
   ├── .fastq.gz
   └── samplesheet.csv
```

- **README.md**: Long description of the assay in markdown format. It should contain provenance of the raw NGS data (samples, laboratory protocols used, aim of the assay, etc)
- **metadata.yml**: metadata file for the assay describing different keys and important information regarding that assay ([see this lesson](./07_metadata.md)).
- **pipeline.md**: description of the pipeline used to process raw data, as well as the commands used to run the pipeline.
- **processed**: folder with results of the preprocessing pipeline. Contents depend on the pipeline used.
- **raw**: folder with the raw data.
    - *.fastq.gz*:In the case of NGS assays, there should be fastq files.
    - *samplesheet.csv*: file that contains metadata information for the samples. This file is used to run the nf-core pipelines. You can also add extra columns with info regarding the experimental variables and batches so it can be used for downstream analysis as well.

## Project folder

On the other hand, we have the other type of folder called `Projects`. In this folder you will save a **subfolder for each project** that you (or your lab) works on. Each `Project` subfolder will contain project information and all the data analysis notebooks and scripts used in that project.

As like for an Assay folder, the Project folder should be named in a way that it is unique, easily readable, distinguishable and understood at a glance. For example, you could name it after the main author initials, a keyword that represents a unique descriptive element of that assay, and the date:

```bash
<author_initials>_<keyword>_YYYYMMDD
```

For example, `JARH_Oct4_20230101`, is a project about the gene Oct4 owned by Jose Alejandro Romero Herrera, created on the 1st of January of 2023.

### Project folder structure

Next, let's take a look at a possible folder structure and what kind of files you can find there.

```bash
<author_initials>_<keyword>_YYYYMMDD
├── data
│  └── <Assay-ID>_<keyword>_YYYYMMDD/
├── documents
│  └── Non-sensitive_NGS_research_project_template.docx
├── notebooks
│  └── 01_data_analysis.rmd
├── README.md
├── reports
│  ├── figures
│  │  └── 01_data_analysis/
│  │   └── heatmap_sampleCor_20230102.png
│  └── 01_data_analysis.html
├── requirements.txt
├── results
│  └── 01_data_analysis/
│      └── DEA_treat-control_LFC1_p01.tsv
├── scripts
└── metadata.yml
```

- **data**: folder that contains symlinks or shortcuts to where the data is, avoiding copying and modification of original files.
- **documents**: folder containing word documents, slides or pdfs related to the project, such as explanations of the data or project, papers, etc. It also contains your [Data Management Plan](./05_DMP.md).
    - *Non-sensitive_NGS_research_project_template.docx*. This is a pre-filled Data Management Plan based on the Horizon Europe guidelines.
- **notebooks**: folder containing Jupyter, R markdown or Quarto notebooks with the actual data analysis.
- **README.md**: detailed description of the project in markdown format.
- **reports**: notebooks rendered as html/docx/pdf versions, ideal for sharing with colleagues and also as a formal report of the data analysis procedure.
    - *figures*: figures produced upon rendering notebooks. The figures will be saved under a subfolder named after the notebook that created them. This is for provenance purposes so we know which notebook created which figures.
- **requirements.txt**: file explaining what software and libraries/packages and their versions are necessary to reproduce the code.
- **results**: results from the data analysis, such as tables with differentially expressed genes, enrichment results, etc.
- **scripts**: folder containing helper scripts needed to run data analysis or reproduce the work of the folder
- **description.yml**: short description of the project.
- **metadata.yml**: metadata file for the assay describing different keys ([see this lesson](./07_metadata.md)).

### Template engine

It is very easy to create a folder template using [cookiecutter](https://github.com/cookiecutter/cookiecutter). Cookiecutter is a command-line utility that creates projects from cookiecutters (that is, a template), e.g. creating a Python package project from a Python package project template. Here you can find an example of a cookiecutter folder template directed to [NGS data](https://github.com/brickmanlab/ngs-template), where we have applied the structures explained in the previous sections. You are very welcome to adapt it or modify it to your needs!

!!! question "Exercise 1: Create your own template"

    Using cookiecutter, create your own templates for your folders. You do not need to copy exactly our suggestions, adjust your template to your own needs!

    We have prepared already two simple cookiecutter templates in GitHub repositories.

    **Assay**

    1. First, fork our `https://github.com/hds-sandbox/assay-template` from the GitHub page into your own account/organization. 
    2. Then, use `git clone` to put it in your computer.
    3. Modify the contents of the repository so that it matches the **Assay** example above. You are welcome to do changes as you please!
    4. Modify the `cookiecutter.json` file so that it will include the Assay name template
    5. Git add, commit and push your changes
    6. Test your folder by using `cookiecutter <URL to your GitHub repository for "assay-template>`
    
    **Project**
    
    1. First, fork our `https://github.com/hds-sandbox/assay-template` from the GitHub page into your own account/organization. 
    2. Then, use `git clone` to put it in your computer.
    3. Modify the contents of the repository so that it matches the **Project** example above. You are welcome to do changes as you please!
    4. Modify the `cookiecutter.json` file so that it will include the **Project** name template
    5. Git add, commit and push your changes
    6. Test your folder by using `cookiecutter <URL to your GitHub repository for "project-template>`

## 2. Metadata and naming conventions

Metadata is the behind-the-scenes information that makes sense of data and gives context and structure. For NGS data, metadata includes information such as when and where the data was collected, what it represents, and how it was processed. Let's check what kind of relevant metadata is available for NGS data and how to capture it in your Assay or Project folders. Both of these folders contain a metadata.yml file and a README.md file. In this section, we will check what kind of information you should collect in each of these files.

### README.md file

The README.md file is a [markdown file](https://www.markdownguide.org/) that allows you to write a long description of the data placed in a folder. Since it is a markdown file, you are able to write in rich text format (bold, italic, include links, etc) what is inside the folder, why it was created/collected, how and when. If it is an `Assay` folder, you could include the laboratory protocol used to generate the samples, images explaining the experiment design, a summary of the results of the experiment and any sort of comments that would help to understand the context of the experiment. On the other hand, a 'Project' README file may contain a description of the project, what are its aims, why is it important, what 'Assays' is it using, how to interpret the code notebooks, a summary of the results and, again, any sort of comments that would help to understand the project.

Here is an example of a README file for a `Project`` folder:

```
# NGS Analysis Project: Exploring Gene Expression in Human Tissues

## Aims

This project aims to investigate gene expression patterns across various human tissues using Next Generation Sequencing (NGS) data. By analyzing the transcriptomes of different tissues, we seek to uncover tissue-specific gene expression profiles and identify potential markers associated with specific biological functions or diseases.

## Why It's Important

Understanding tissue-specific gene expression is crucial for deciphering the molecular basis of health and disease. Identifying genes that are uniquely expressed in certain tissues can provide insights into tissue function, development, and potential therapeutic targets. This project contributes to our broader understanding of human biology and has implications for personalized medicine and disease research.

## Datasets

We have used internal datasets with IDs: RNA_humanSkin_20201030, RNA_humanBrain_20210102, RNA_humanLung_20220304.

In addition, we utilized publicly available NGS datasets from the GTEx (Genotype-Tissue Expression) project, which provides comprehensive RNA-seq data across multiple human tissues. These datasets offer a wealth of information on gene expression levels and isoform variations across diverse tissues, making them ideal for our analysis.

## Summary of Results

Our analysis revealed distinct gene expression patterns among different human tissues. We identified tissue-specific genes enriched in brain tissues, highlighting their potential roles in neurodevelopment and function. Additionally, we found a set of genes that exhibit consistent expression across a range of tissues, suggesting their fundamental importance in basic cellular processes.

Furthermore, our differential expression analysis unveiled significant changes in gene expression between healthy and diseased tissues, shedding light on potential molecular factors underlying various diseases. Overall, this project underscores the power of NGS data in unraveling intricate gene expression networks and their implications for human health.

---

For more details, refer to our [Jupyter Notebook](link-to-jupyter-notebook.ipynb) for the complete analysis pipeline and code.
```

### metadata.yml

The metadata file is a [yml file](https://fileinfo.com/extension/yml), which is a text document that contains data formatted using a human-readable data format for data serialization.

![yaml file example](./images/yml_example.png)

### Metadata fields

There is a ton of information you can collect regarding an NGS assay or a project. Some information fields are very general, such as author or date, while others are specific to the Assay or Project folder. Below, we will take a look at minimal information you should collect in each of the folders.

#### General metadata fields

Here you can find a list of suggestions for general metadata fields that can be used for both assays and project folders:

- **Title**: A brief yet informative name for the dataset.
- **Author(s)**: The individual(s) or organization responsible for creating the dataset. You can use your [ORCID](https://orcid.org/)
- **Date Created**: The date when the dataset was originally generated or compiled. Use YYYY-MM-DD format!
- **Description**: A short narrative explaining the content, purpose, and context.
- **Keywords**: A set of descriptive terms or phrases that capture the folder's main topics and attributes.
- **Version**: The version number or identifier for the folder, useful for tracking changes.
- **License**: The type of license or terms of use associated with the dataset/project.

#### Assay metadata fields

Here you will find a table with possible metadata fields that you can use to annotate and track your `Assay` folders:

{{ read_table('./assets/assay_metadata.tsv') }}

#### Project metadata fields

Here you will find a table with possible metadata fields that you can use to annotate and track your `Project` folders:

{{ read_table('./assets/project_metadata.tsv') }}

### More info

The information provided in this lesson is not at all exhaustive. There might be many more fields and controlled vocabularies that could be useful for your NGS data. We recommend that you take a look at the following sources for more information!

- [Transcriptomics metadata standards and fields](https://faircookbook.elixir-europe.org/content/recipes/interoperability/transcriptomics-metadata.html#analysis-metadata)
- [Bionty](https://lamin.ai/docs/bionty): Biological ontologies for data scientists.

!!! question "Exercise 2: modify the metadata.yml files in your cookiecutter templates"

    We have seen some examples of metadata for NGS data. It is time now to customize your cookiecutter templates and modify the metadata.yml files so that they fit your needs! 
    
    1. Think about what kind of metadata you would like to include and add them to your metadata.yml file from your cookiecutter template. 
    2. Modify the `cookiecutter.json` file so that when you create a new folder template, all the metadata is filled accordingly.
    3. Modify the `metadata.yml` file so that it includes the metadata recorded by the `cookiecutter.json` file.
    4. Modify the `README.md` file so that it includes the short description recorded by the `cookiecutter.json` file.
    5. Git add, commit and push the changes
    6. Test your folders by using the command `cookiecutter <URL to your cookiecutter repository in GitHub>`

## 3. Naming conventions

Using consistent naming conventions is important in scientific research as it helps with the organization and retrieval of data or results. By adopting standardized naming conventions, researchers ensure that files, experiments, or data sets are labeled in a clear, logical manner. This makes it easier to locate and compare similar types of data or results, even when dealing with large datasets or multiple experiments. For instance, in genomics, employing uniform naming conventions for files related to specific experiments or samples allows for swift identification and comparison of relevant data, streamlining the research process and contributing to the reproducibility of findings. This practice promotes efficiency, collaboration, and the integrity of scientific work.

### General tips

Below you will find a small list of general tips to follow when you name a folder or a file:

- Use only alphanumeric characters to write a word: a to z and 0 to 9
- Avoid special characters: ~!@#$%^&*()`[]{}"|
- Date format: use `YYYYMMDD` format. For example: 20230101.
- Authors: use initials. For example: JARH
- **Don't use of spaces**! Computers get very confused when you need to point a path to a file and it contains spaces! Instead:
    - Separate field sections are separated by underscores `_`.
    - Words in each section are written in [camelCase](https://en.wikipedia.org/wiki/Camel_case).
  It would look then like this: `field1_word1Word2.txt`. For example: `heatmap_sampleCor_20230101.png`. The first field indicates what this file is, i.e., a heatmap. Second field is what is being plotted, i.e, sample correlations; since the field contains two words, they are written in camelCase. The third field is the date of when the image was created.
- Use as short fields as possible. You can try to use understandable abbreviations, like LFC for LogFoldChange, Cor for correlations, Dist for distances, etc.
- Avoid long names as much as you can, be concise!
- Avoid creating many sublevels of folders.
- Write down your naming convention pattern and document it in the README file
- When using a sequential numbering system, use leading zeros to make sure files sort in sequential order. Using `01` instead of just `1` if your sequence only goes up to `99`.
- Versions should be used as the last element, and use at least two digits with a leading 0 (e.g. v01, v02)

### Suggestions for NGS data

More info on naming conventions for different types of files and analysis is in development.

{{ read_table('./assets/file_naming_convention.tsv') }}

!!! question "Exercise 3: Create your own naming conventions"

    Think about the most common types of files and folders you will be working on, such as visualizations, results tables, processed files, etc. Then come up with a logical and clear way of naming those files using the tips suggested above. Remember to avoid making long and complicated names!

## 4. Create a catalog of your assays folder



## 5. Version control of your data analysis using Git and Github

Version control is a systematic approach to tracking changes made to a project over time. It provides a structured means of documenting alterations, allowing you to revisit and understand the evolution of your work. In research data management and data analytics, version control is very important and gives you a lot of advantages.

[Git](https://git-scm.com/about) is a distributed version control system that enables developers and researchers to efficiently manage their project's history, collaborate seamlessly, and ensure data integrity. At its core, Git operates through the following principles and mechanisms:
On the other hand, [GitHub](https://github.com/) is a web-based platform that enhances Git's capabilities by providing a collaborative and centralized hub for hosting Git repositories. It offers several key functionalities, such as tracking issues, security features to safeguard your repos, and GitHub Pages that allows you to create websites to showcase your projects.

!!! tip "Create a GitHub organization for your lab or department"

    GitHub allows users to create organizations and teams that will collaborate together or create repositories under the same umbrella organization. If you would like to create an educational organization in GitHub, you can do so for free! For example, you could create a GitHub account for your lab.

    In order to create a GitHub organization, follow these [instructions](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch)

    After you have created the GitHub organization, make sure that you create your repositories under the organization space and not your own user!

### Creating a git repo online and copying your project folder

If the repository already exists on a remote, you would choose to `git clone` and not `git init`. On the other hand, if you create a remote repository first with the intent of moving your project to it later, you may have a few other steps to follow. If there are no commits in the remote repository, you can follow the steps above for `git init`. If there are commits and files in the remote repository but you would still like it to contain your project files, `git clone` that repository. Then, move the project's files into that cloned repository. `git add`, `git commit`, and `git push` to create a history that makes sense for the beginning of your project. Then, your team can interact with the repository without `git init` again.

!!! tip "Tips to write good commit messages"

    If you would like to know more about Git commits and the best way to make clear git messages, check out [this post](https://www.conventionalcommits.org/en/v1.0.0/)!

### GitHub Pages

Once you have created your repository (and put it in GitHub), you have now the opportunity to add your data analysis reports that you created, in either Jupyter Notebooks, Rmarkdowns or html reports, in a [GitHub Page website](https://pages.github.com/). Creating a GitHub page is very simple, and we really recommend that you follow the nice tutorial that GitHub as put for you.

There are many different ways to create your webpages. We recommend using Mkdocs and Mkdocs materials as a framework to create a nice webpage in a simple manner. The folder templates that we used as an example in [lesson 06](./06_file_structure.md) already contain everything you need to start a webpage. Nonetheless, you will need to understand the basics of [MkDocs](https://www.mkdocs.org/) and [MkDocs materials](https://squidfunk.github.io/mkdocs-material/) to design a webpage to your liking. MkDocs is a static webpage generator that is very easy to use, while MkDocs materials is an extension of the tool that gives you many more options to customize your website. Check out their webpages to get started!

!!! question "Exercise 4: make a webpage"

### Configure your main GitHub Page and its repo

The next step is to set up the main GitHub Page site and the repository that will host it. This is very simple, as you will only need to follow [these steps](https://pages.github.com/).

After you have created the *organization*github.io, it is time to configure your webpage using MkDocs!

#### Use mkdocs to create your webpage

Follow the steps on the [MkDocs documentation](https://www.mkdocs.org/getting-started/) to get started on your webpage! You can use a simple markdown file describing your organization (your lab or department), its main goals and missions and maybe a couple of images showcasing your research.

When you are happy with your webpage and are ready too publish it, make sure to add, commit and push the changes to the remote! Instead of using the basic setup that GitHub offers, we recommend that you build up your webpage using MkDocs and the [`mkdocs gh-deploy`](https://www.mkdocs.org/user-guide/deploying-your-docs/) command! This requires a couple of changes in your GitHub organization settings.

#### Publishing your GitHub Page

Go to your GitHub organization settings and configure the Page section. Since you are using the `mkdocs gh-deploy` command to publish your site in the `gh-pages` branch (as explained the the mkdocs documentation), we need to change where GitHub is fetching the website from:

![GitHub Pages setup](./images/git_pages.png)

- Branch should be `gh-pages`
- Folder should be `root`

After a couple of minutes, your webpage should be ready!

### Start a new project from cookiecutter

Using cookiecutter, create a new data analysis project. Remember to fill up your metadata and description files! After you have created the folder, it would be best to initialize a Git repo following the instructions from the [previous section](#creating-a-git-repo-from-an-existing-folder).

Next, link your data of interest and make an example of data analysis notebook/report. Depending on your setup, you might be using Jupyter Notebooks or Rmarkdowns. The extensions that we have installed using `pip` allows you to directly add a Jupyter Notebook file to the `mkdocs.yml` navigation section. On the other hand, if you are using Rmarkdown, you will have to knit your document into either an html page or a github document.

### Publishing your project as a GitHub Page

Remember to make sure that your markdowns, images, reports, etc., are included in the `docs` folder and properly set up in the navigation section of your `mkdocs.yml` file.

Git add, commit and push your changes. Then, run `mkdocs gh-deploy`. You will still need to configure the settings of this repositories in GitHub, so that the Page is taken from the `gh-pages` branch and the `root` folder. You should be able to see your webpage through the link provided in the Page section!

Now it is also possible to include this repository webpage in your main webpage *organization*github.io by including the link of the repo website (https://*organization*github.io/*repo-name*) in the navigation section of the `mkdocs.yml` file in the main *organization*github.io repo.

## 6. Archive GitHub repositories on Zenodo

In this lesson, we're going to explore repositories like Zenodo, Gene Expression Omnibus, and Annotare. While platforms like GitHub are great for version control and collaborative coding, these repositories serve a different purpose. They're designed specifically for archiving and sharing scientific data, ensuring it's preserved for the long term and accessible to the global research community. You could think of them as secure digital libraries for your valuable NGS data.

### What is a repository/archive?

Specialized repositories/archives are dedicated digital platforms designed for the secure storage, curation, and dissemination of scientific data. These repositories hold great importance in the research community as they serve as reliable archives for preserving valuable datasets. Their standardized formats and robust curation processes ensure the long-term accessibility and citability of research findings. Researchers worldwide rely on these repositories to share, discover, and validate scientific information, thereby fostering transparency, collaboration, and the advancement of knowledge across various domains of study.

#### NGS Data upload to GEO or Annotare

[GEO](https://www.ncbi.nlm.nih.gov/geo/) and [Annotare](https://www.ebi.ac.uk/fg/annotare/login/) are excellent repository choices to deposit your NGS data. Both Annotare and GEO adhere to established community standards for data submission and sharing in the field of functional genomics:

1. [**Minimum Information About a Microarray Experiment (MIAME)**](https://pubmed.ncbi.nlm.nih.gov/11726920/): This is a set of guidelines established to ensure the comprehensive and standardized reporting of microarray experiments. Both Annotare and GEO require compliance with MIAME standards for microarray data submissions.
2. [**Minimum Information about a high-throughput SeQuencing Experiment (MIxS)**](https://www.fged.org/projects/minseqe/): MIxS is a set of standards developed by the Genomic Standards Consortium to ensure consistent reporting of metadata for high-throughput sequencing experiments. Annotare and GEO require adherence to MIxS standards for sequencing data submissions.
3. [**Sequence Read Archive (SRA) Submission Guidelines**](https://www.ncbi.nlm.nih.gov/sra/docs/submit/): Both Annotare and GEO follow the submission guidelines set forth by the Sequence Read Archive, which include requirements for data formatting, metadata inclusion, and quality control.
4. **Community-Specific Standards**: In addition to the above, Annotare and GEO may also adhere to community-specific standards and guidelines established by the functional genomics research community. These standards are designed to ensure that submitted data meets the specific requirements and expectations of the field.

By adhering to these standards, Annotare and GEO ensure that the data submitted to their repositories is of high quality, well-documented, and compliant with community best practices. This facilitates data discovery, reproducibility, and interoperability within the scientific community.

These repositories will only accept NGS data and information related to the creation of the data. This includes the raw FASTQ files, sample metadata, including protocols and descriptions of how the samples and data where processed, as well as final pre-processing results such as read count matrices or genomic position files (like BED). If you adhere to the `Assay` folder creation guideline of [lesson 6](./06_file_structure.md), you will have a very easy time filling up the required documentation and information needed to submit the data in your `Assay` folder to one of these repositories.

Nonetheless, the repositories will not accept other data created by your down-stream analyses, neither the code used for data analyses! This means anything that you have done in your `Project` folder. However, your `Project` folder is already version controlled by GitHub ([see previous lesson](./09_version_control.md)), so there is no need to worry. We will see in the section below how to archive your `Project`` folder as well using a general repository like Zenodo.

### Zenodo

Zenodo[https://zenodo.org/] is an open-access digital repository designed to facilitate the archiving of scientific research outputs. It operates under the umbrella of the European Organization for Nuclear Research (CERN) and is supported by the European Commission. Zenodo accommodates a broad spectrum of research outputs, including datasets, papers, software, and multimedia files. This versatility makes it an invaluable resource for researchers across a wide array of domains, promoting transparency, collaboration, and the advancement of knowledge on a global scale.

Operating on a user-friendly web platform, Zenodo allows researchers to easily upload, share, and preserve their research data and related materials. Upon deposit, each item is assigned a unique Digital Object Identifier (DOI), granting it a citable status and ensuring its long-term accessibility. Additionally, Zenodo provides robust metadata capabilities, enabling researchers to enrich their submissions with detailed contextual information. In addition, it allows you to [link you GitHub account](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content), providing a streamlined way to archive a specific release of your GitHub repository directly into Zenodo. This integration simplifies the process of preserving a snapshot of your project's progress for long-term accessibility and citation.

#### `Project` archiving in Zenodo

Once your [accounts are linked](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content), creating a Zenodo archive is as simple as tagging a release in your GitHub repository. Zenodo will automatically detect the release and generate a corresponding archive. This archive is assigned a unique Digital Object Identifier (DOI), making it a citable reference for your work. So, before submitting your work in a journal, make sure to link your data analysis repository to [Zenodo](https://zenodo.org/), get a DOI and cite it in your manuscript!

By leveraging this integration, you ensure that significant milestones in your project are preserved in a reliable and accessible manner. This not only facilitates proper attribution but also contributes to the broader scientific community's ability to reproduce and build upon your research.

## Wrap up