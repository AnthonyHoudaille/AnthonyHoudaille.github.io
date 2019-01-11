This was forked (then detached) by [Stuart Geiger](https://github.com/staeiou) from the [Minimal Mistakes Jekyll Theme](https://mmistakes.github.io/minimal-mistakes/), which is © 2016 Michael Rose and released under the MIT License. See LICENSE.md.

This is a personal Data Science blog focusing on statistics, machine learning, deep learning and big data.

# Add a page

1. Go into "pages" and create and new .md file
1. Set header :  permalink: /collectionname/
1. Include posts content : {% include base_path %} {% for post in site.collectionname reversed %} {% include archive-single.html %} {% endfor %}. Pay attention to the collection name which should be the same as the permalink
1. Create a folder "name" which contains the posts that will go in this section
1. Once created, add pages in it and make sure that the -md files contain : collection: collectionname
1. In data, change the "navigation" file
1. In "config", add the collection :
  collectionname:
    output: true
    permalink: /:collection/:path/

See more info at https://academicpages.github.io/
