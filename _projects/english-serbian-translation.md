---
layout: page
title: English to Serbian Translation
description: Neural machine translation model for English to Serbian language pairs
img: assets/img/translation.png
importance: 2
category: fun
related_publications: false
---

### Project Overview

As machine translation applications continue to expand into the
realm of real-time events, the need for faster and more concise
translation becomes increasingly important. One such application
is simultaneous speech translation, an emission of subtitles in the
target language given speech in the source language. In this work,
we focus on easing reader’s comprehension of subtitles by making
the translation shorter while preserving its informativeness. For
this, we use the small version of the Paraphrase Database (PPDB),
and exploit its property that some of the paraphrasing rules differ
in length of the left and right side. Selecting rules that make the
output shorter, we fine-tune an MT model to naturally generate
shorter translations. The results show that the model’s concise-
ness improves by 0.22%, which leaves the space for
improvements using bigger versions of PPDB in future work.

### Live Demo

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <iframe
            src="https://perkan-shorterengtoserb.hf.space"
            frameborder="0"
            width="100%"
            height="450"
            style="border: 2px solid #000; border-radius: 8px;"
        ></iframe>
    </div>
</div>

For more information about the research methodology, technical details, or to explore similar projects, feel free to reach out!
