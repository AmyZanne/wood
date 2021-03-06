# Moving the frontier of reproducible science forward

Science is reportedly in the middle of a [reproducibility crisis](http://theconversation.com/science-is-in-a-reproducibility-crisis-how-do-we-resolve-it-16998).  Reproducibility seems laudable and is frequently called for (e.g., calls in [nature](http://www.nature.com/nature/focus/reproducibility/) and [science](http://www.sciencemag.org/content/334/6060/1226)).  In general the argument is that research that can be independently reproduced is more reliable than research that cannot be independently reproduced.  It is also worth noting that reproducing research is not solely a checking process, and it can provide useful jumping-off points for future research questions.  It is difficult to find a counter-argument to these claims, but arguing that reproducibility is laudable in general glosses over the fact that for each research group it is a significant amount of work to make their research (easily) reproducible for independent scientists.     While much of the attention has focussed on [entirely repeating laboratory experiments](http://www.nature.com/nature/journal/v483/n7391/full/483531a.html), there is are many simpler forms of reproducibility including, for example, the ability to recompute analyses on known sets of data.

Different types of scientific research are inherently easier or harder to reproduce.  At one extreme are analytic mathematical research, which should in many cases allow for straightforward reproduction based on the equations in the manuscript.  At the other extreme are field-based studies, which may depend upon factors that are not under the control of the scientist. To use an extreme example, it will always be effectively impossible to entirely reproduce a before and after study of the effects of a hurricane.

The current location of the frontier of reproducibility is somewhere between these two extreme examples, and the location of this frontier depends upon the set of tools available to researchers at any given time.  Open source software, cloud data archiving, standardized biological materials, and widely available computing resources have all pushed this frontier to allow for the reproduction of *more types* of research than was previously the case.   However, the  ["reproducibility crisis"](http://theconversation.com/science-is-in-a-reproducibility-crisis-how-do-we-resolve-it-16998) rhetoric suggests that this set of tools is, while substantial, incomplete.

We recently worked on a project that we believe is close to the current "frontier of reproducibility"--a moderately complex analysis of a moderately sized database ([49061 rows](https://datadryad.org/resource/doi:10.5061/dryad.63q27)). (This was to answer a very simple research question: [what proportion of the world's plant species are woody?](http://onlinelibrary.wiley.com/doi/10.1111/1365-2745.12260/abstract).) Our specific experiences in trying to make this research reproducible may be useful for the on-going discussion of how to allow scientists to make their research reproducible with less time and fewer technical skills.  In other words, how do we usefully move the "frontier of reproducibility" to include more types of studies and in doing so make more science more reliable.

In the end, our analysis and paper has been reproduced independently and it is relatively easy for anyone who wants to do so, but this level of reproducibility was not without considerable effort.  For those interested, the entirety of our the code and documentation is available [here](https://github.com/richfitz/wood).

There are two parts to the reproducibility of a project like this: the data and the analysis. We should note that the fact that this project was even possible is due to the [recent open developments in data archiving](http://en.wikipedia.org/wiki/Scientific_data_archiving).  It was relatively straightforward to write a script that downloads the main data from [Dryad](http://datadryad.org/) and prepares it for analysis.  However, this proved to be only the beginning of the challenge: the analysis portion turned out to be much more challenging.  The following is essentially a list of lessons learned from that experience.  Each bullet point below is one of the challenges we faced in making our research reproducible and the tool we chose to address that challenge.

## Challenges and tools for reproducibility

**Using canonical data sources**

We downloaded data from canonical sources ([Dryad](http://datadryad.org) and [The Plant List](http://theplantlist.org) and only modified them programmatically, so that the chain of modification is preserved.  The benefits of open data will only be realised if we preserve identity of data sets and not end up re-archiving hundreds of slightly modified version.  This also helps ensure credit for data contributors.

**Combining thoughts and code**

We used [knitr](http://yihui.name/knitr/) to implement the analysis in a [literate programming](http://en.wikipedia.org/wiki/Literate_programming) style.  The entire analysis, including justification of the core functions, is [available to interested people](http://richfitz.github.io/wood/wood.html).

**Dynamic generation of figures**

All of our data manipulation was handled with scripts, and we could delete all figures/outputs and recreate them at will.

**Version control**

All our scripts were under version control using [git](http://git-scm.com) from [the beginning](https://github.com/richfitz/wood/commit/8ed0c8c10dfda2a8f11f169ec528b7e161832eeb), enabling us to dig back through old versions.  This was central to everything we did!  See [this article](http://www.scfbm.org/content/8/1/7) for much more on how version control facilitates research.

**Automated checking that modifications don't break things**

We used the "[continuous integration](http://en.wikipedia.org/wiki/Continuous_integration)" environment [Travis CI](http://travis-ci.org) to guard against changes in the analysis causing it to fail.  Every time we made a change, this system downloads the source code, all relevant data and runs the analysis, sending us an email if anything failed.  It even [uploads the compiled versions of the analysis and manuscript](http://richfitz.github.io/wood) each time it runs.

**Documenting dependencies**

We used [packrat](https://github.com/rstudio/packrat)` for managing and archiving R package dependencies to ensure future repeatability.  In theory, this means that if software versions change enough to break our scripts, we have an archived set of packages that can be used.  This is a very new tool; only time will tell if this will work, however.

## Remaining challenges

We found that moving from running analyses on one person's computer (with their particular constellation of software locations) to another was difficult. For example, see [this issue](https://github.com/richfitz/wood/issues/1) for the trouble that we had getting the analyses run on our own computers, knowing the scope of the project.  It's hard to anticipate all possible causes for confusion: one initial try at replication by [Carl Boettiger](http://carlboettiger.info) had [trouble](https://github.com/richfitz/wood/issues/12) due to incomplete documentation of required package versions.

The set of scripts that manages the above jobs is comparable in size to the actual analysis; this is a large overhead to place on researchers.  Automating as much of this as possible is going to be essential for this type of reproducibility to become standard practice.  There is also many different languages and frameworks involved, increasing both the technical knowledge required and the chance that something will break.

The continous integration approach has a huge potential to save headaches in managing computational research projects.  However, while our analysis acts as a proof-of-concept, it will be of limited general use: it requires that the project is open source (in a *public* [github](https://github.com) repository), and that the analysis is relatively quick to run (under an hour).  These limitations are reasonable given that it is a free service, but don't match well onto many research projects where development does not occur "in the open", and where computation can take many hours or days.

---

We found our reproducibility goals for this paper to be a useful exercise, and it forms the basis of [ongoing research projects](https://github.com/richfitz/modeladequacy).  However, the process is far too complicated at the moment. It is not going to be enough to simply tell people to make their projects reproducible. We need to develop tools that are *at least* as easy to use as version control before we can expect project reproducibility to become mainstream.

Other efforts for this simple goal of recomputibility are not much more encouraging than our efforts.  A study in the [UBC Reproducibility Group](http://www.zoology.ubc.ca/~repro) found that they [could not reproduce the results in 30%](http://onlinelibrary.wiley.com/doi/10.1111/j.1365-294X.2012.05754.x/abstract) of published analyses using the population genetic package STRUCTURE, using the same data as provided by the authors.  In an even more trivial case, a [research group at Arizona university](http://reproducibility.cs.arizona.edu/) found that they could only *build* about half of the published software that they could download, without even testing that the software did what it was intended to do (note that this study is currently [being reproduced!](http://cs.brown.edu/~sk/Memos/Examining-Reproducibility/)).

Despite considerable effort, we are only part of the way there.
