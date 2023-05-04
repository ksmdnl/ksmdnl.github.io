<!-- ---
permalink: /
title: "Daniel Kusuma"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---
I am a master student at RWTH Aachen in electrical engineering, pursing my deep interest and curiosity in deep learning. During my bachelor study I was working with [Andreas Bär](https://andrbaer.github.io/) under the supervision of [Prof. Dr.-Ing. Tim Fingscheidt](https://www.tu-braunschweig.de/ifn/institut/abt/sv/prof-dr-ing-tim-fingscheidt).


News
------
- dsamdklsamdksamdklasda
- ndjksandjsandjsandjsadnjsak
Create content & metadata
------
For site content, there is one markdown file for each type of content, which are stored in directories like _publications, _talks, _posts, _teaching, or _pages. For example, each talk is a markdown file in the [_talks directory](https://github.com/academicpages/academicpages.github.io/tree/master/_talks). At the top of each markdown file is structured data in YAML about the talk, which the theme will parse to do lots of cool stuff. The same structured data about a talk is used to generate the list of talks on the [Talks page](https://academicpages.github.io/talks), each [individual page](https://academicpages.github.io/talks/2012-03-01-talk-1) for specific talks, the talks section for the [CV page](https://academicpages.github.io/cv), and the [map of places you've given a talk](https://academicpages.github.io/talkmap.html) (if you run this [python file](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.py) or [Jupyter notebook](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.ipynb), which creates the HTML for the map based on the contents of the _talks directory).

**Markdown generator**

I have also created [a set of Jupyter notebooks](https://github.com/academicpages/academicpages.github.io/tree/master/markdown_generator
) that converts a CSV containing structured data about talks or presentations into individual markdown files that will be properly formatted for the academicpages template. The sample CSVs in that directory are the ones I used to create my own personal website at stuartgeiger.com. My usual workflow is that I keep a spreadsheet of my publications and talks, then run the code in these notebooks to generate the markdown files, then commit and push them to the GitHub repository.

Publications
------
- Improvements to Image Reconstruction-Based Performance Prediction\\for Semantic Segmentation in Highly Automated Driving
Example: editing a markdown file for a talk
![Editing a markdown file for a talk](/images/editing-talk.png)

<h2>Selected Publications</h2>

<div class="paper-entry">
  <a href="https://point2vec.ka.codes/" class="special-link">
    <p>
      <video style="padding-bottom: 0px; margin-top: -30px;" class="align-left teaser" autoplay muted loop>
        <source src="https://jonasschult.github.io/images/teaser/point2vec.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <b>Improvements to Image Reconstruction-Based Performance Prediction for Semantic Segmentation in Highly Automated Driving</b>
      </br>
      Andreas Bär, <u>Daniel Kusuma</u>*, Tim Fingscheidt
      </br>
      arXiv, 2023.
      </br>
      <a class="btn btn--research" href="https://github.com/ifnspaml/PerfPredRecV2/">
        <span class="icon" style="margin-right: 0em;">
          <i class="fas fa-bolt"></i>
        </span>
        <span>Source Code</span>
      </a>
      <!-- <a class="btn btn--research" href="https://arxiv.org/abs/2303.16570">
        <span class="icon" style="margin-right: 0em;">
          <i class="fas fa-file-pdf"></i>
        </span>
        <span>Paper</span>
      </a> -->
      <a class="btn btn--research collapsible" id="collapse_point2vec">
        <span class="icon" style="margin-right: 0em;">
          <i class="fas fa-book"></i>
        </span>
        <span>BibTex</span>
      </a>
      <a class="btn btn--research" href="https://point2vec.ka.codes/">
        <span class="icon" style="margin-right: 0em;">
          <i class="fab fa-github"></i>
        </span>
        <span>Code</span>
      </a>
    </p>
  </a>
  <div id="content_point2vec" class="collapsible-content">
    <pre class="p-bibtex">
{% raw %}
@article{abouzeid2023point2vec,
  title={Point2Vec for Self-Supervised Representation Learning on Point Clouds},
  author={Abou Zeid, Karim and Schult, Jonas and Hermans, Alexander and Leibe, Bastian},
  journal={arXiv preprint arXiv:2303.16570},
  year={2023},
}
{% endraw %}
    </pre>
  </div>
</div>
<script>
  var coll = document.getElementsByClassName("collapsible");
  var i;

  for (i = 0; i < coll.length; i++) {
    coll[i].addEventListener("click", function () {
      id = this.id;
      var content = document.getElementById(id.replace("collapse", "content"));
      if (content.style.maxHeight) {
        content.style.maxHeight = null;
      } else {
        content.style.maxHeight = content.scrollHeight + "px";
      }
    });
  }
</script> -->