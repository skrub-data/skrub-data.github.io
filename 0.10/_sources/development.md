# Development

While `skrub` is still in its early stages, we believe in openness and
community development from the start. Join us in building a great package to
facilitate learning on databases.

<!-- File to ..include in a document with a big table of content, to give
it 'style' --> <script>
 window.addEventListener('DOMContentLoaded', function() {
      (function($) {
 //Function to make the index toctree collapsible
 $(function () {
     $('main .toctree-l2')
         .click(function(event){
             if (event.target.tagName.toLowerCase() != "a") {
                 if ($(this).children('ul').length > 0) {
                      $(this).attr('data-content',
                          (!$(this).children('ul').is(':hidden')) ? '\\u25ba' : '\\u25bc');
                     $(this).children('ul').toggle();
                 }
                 return true; //Makes links clickable
             }
         })
         .mousedown(function(event){ return false; }) //Firefox highlighting fix
         .children('ul').hide();
     // Initialize the values
     $('main li.toctree-l2:not(:has(ul))').attr('data-content', '-');
     $('main li.toctree-l2:has(ul)').attr('data-content', '\\u25ba');
     $('main li.toctree-l2:has(ul)').css('cursor', 'pointer');

     $('main .toctree-l2').hover(
         function () {
             if ($(this).children('ul').length > 0) {
                 $(this).css('background-color', '#88888844');
                 $(this).attr('data-content',
                     (!$(this).children('ul').is(':hidden')) ? '\\u25bc' : '\\u25ba');
             }
             else {
                 $(this).css('background-color', '#88888844');
             }
         },
         function () {
             $(this).css('background-color', '#88888800');
             if ($(this).children('ul').length > 0) {
                 $(this).attr('data-content',
                     (!$(this).children('ul').is(':hidden')) ? '\\u25bc' : '\\u25ba');
             }
         }
     );
 });
      })(jQuery);
  });
 </script>

<style type="text/css">
  main li, div.body ul {
      transition-duration: 0.2s;
  }

  main li.toctree-l1 {
      padding: 5px 0 0;
      list-style-type: none;
      font-size: 150%;
      font-weight: normal;
      margin-left: 0;
      margin-bottom: 1.2em;
      font-weight: bold;
      }

  main li.toctree-l1 > a {
      margin-left: 0px;
  }

  main li.toctree-l1 ul {
      padding-inline-start: 0em;
  }

  main li.toctree-l2 {
      padding: 0.25em 0 0.25em 0 ;
      list-style-type: none;
      font-size: 85% ;
      font-weight: normal;
      margin-left: 0;
  }

  main li.toctree-l2 ul {
      padding-left: 30px ;
  }

  main li.toctree-l2:before {
      content: attr(data-content);
      font-size: 1rem;
      color: #777;
      display: inline-block;
      width: 1.5rem;
      margin-left: -1.5rem;
  }

  main li.toctree-l3 {
      font-size: 88% ;
      font-weight: normal;
      margin-left: 0;
  }

  main li.toctree-l4 {
      font-size: 93% ;
      font-weight: normal;
      margin-left: 0;
  }

  main div.topic li.toctree-l1 {
      font-size: 100% ;
      font-weight: bold;
      background-color: transparent;
      margin-bottom: 0;
      margin-left: 1.5em;
      display:inline;
  }

  main div.topic p {
      font-size: 90% ;
      margin: 0.4ex;
  }

  main div.topic p.topic-title {
      display:inline;
      font-size: 100% ;
      margin-bottom: 0;
  }

  min li {
      list-style-type: none;
  }

  main div.toctree-wrapper ul {
      padding-left: 0;
  }

  main li.toctree-l1 {
      padding: 0 0 0.5em 0;
      font-size: 150%;
      font-weight: bold;
  }

  main li.toctree-l2 {
      font-size: 70%;
      font-weight: normal;
      margin-left: 30px;
  }

  main li.toctree-l3 {
      font-size: 85%;
      font-weight: normal;
      margin-left: 30px;
  }

  main li.toctree-l4 {
      margin-left: 30px;
  }

</style>

* [Vision: Where is skrub heading?](visionhtml.md)
  * [Vision Statement](visionhtml.md#vision-statement)
  * [Roadmap](visionhtml.md#roadmap)
* [About](abouthtml.md)
* [Contributing guide](CONTRIBUTINGhtml.md)
  * [Have a question?](CONTRIBUTINGhtml.md#have-a-question)
  * [What to know before you begin](CONTRIBUTINGhtml.md#what-to-know-before-you-begin)
  * [How can I contribute?](CONTRIBUTINGhtml.md#how-can-i-contribute)
    * [Reporting bugs](CONTRIBUTINGhtml.md#reporting-bugs)
      * [How to submit a bug report?](CONTRIBUTINGhtml.md#how-to-submit-a-bug-report)
      * [How to write an example?](CONTRIBUTINGhtml.md#how-to-write-an-example)
    * [Suggesting enhancements](CONTRIBUTINGhtml.md#suggesting-enhancements)
      * [How to submit an enhancement proposal?](CONTRIBUTINGhtml.md#how-to-submit-an-enhancement-proposal)
        * [If the enhancement proposal is validated](CONTRIBUTINGhtml.md#if-the-enhancement-proposal-is-validated)
        * [If the enhancement is refused](CONTRIBUTINGhtml.md#if-the-enhancement-is-refused)
    * [Writing your first Pull Request](CONTRIBUTINGhtml.md#writing-your-first-pull-request)
      * [Preparing the ground](CONTRIBUTINGhtml.md#preparing-the-ground)
      * [Setting up the environment](CONTRIBUTINGhtml.md#setting-up-the-environment)
      * [Implementation Guidelines](CONTRIBUTINGhtml.md#implementation-guidelines)
  * [Checking the quality of your code contribution](CONTRIBUTINGhtml.md#checking-the-quality-of-your-code-contribution)
    * [Testing the code](CONTRIBUTINGhtml.md#testing-the-code)
    * [Checking coverage on the local machine](CONTRIBUTINGhtml.md#checking-coverage-on-the-local-machine)
    * [Updating doctests](CONTRIBUTINGhtml.md#updating-doctests)
    * [Formatting and pre-commit checks](CONTRIBUTINGhtml.md#formatting-and-pre-commit-checks)
    * [Ensuring the documentation builds](CONTRIBUTINGhtml.md#ensuring-the-documentation-builds)
    * [Editing the API reference documentation](CONTRIBUTINGhtml.md#editing-the-api-reference-documentation)
  * [Submitting your code](CONTRIBUTINGhtml.md#submitting-your-code)
    * [Updating the changelog](CONTRIBUTINGhtml.md#updating-the-changelog)
    * [Continuous Integration (CI)](CONTRIBUTINGhtml.md#continuous-integration-ci)
      * [Integration](CONTRIBUTINGhtml.md#integration)
* [How to write an example for the gallery](tutorial_examplehtml.md)
  * [Location of the examples](tutorial_examplehtml.md#location-of-the-examples)
    * [Dealing with typos in the example](tutorial_examplehtml.md#dealing-with-typos-in-the-example)
  * [Writing the example](tutorial_examplehtml.md#writing-the-example)
  * [Running the example](tutorial_examplehtml.md#running-the-example)
  * [Adding cross-references](tutorial_examplehtml.md#adding-cross-references)
  * [Generating the new documentation](tutorial_examplehtml.md#generating-the-new-documentation)
  * [Linking your work to examples already in the documentation](tutorial_examplehtml.md#linking-your-work-to-examples-already-in-the-documentation)
  * [Merging your example](tutorial_examplehtml.md#merging-your-example)
* [Release process](RELEASE_PROCESShtml.md)
  * [Target audience](RELEASE_PROCESShtml.md#target-audience)
  * [Process](RELEASE_PROCESShtml.md#process)
    * [Preparing the release branch](RELEASE_PROCESShtml.md#preparing-the-release-branch)
    * [Meanwhile, preparing the post-released PR](RELEASE_PROCESShtml.md#meanwhile-preparing-the-post-released-pr)
    * [The doc update has succeeded](RELEASE_PROCESShtml.md#the-doc-update-has-succeeded)
    * [Pushing the wheel to Pypi](RELEASE_PROCESShtml.md#pushing-the-wheel-to-pypi)
    * [Update the conda-forge recipe](RELEASE_PROCESShtml.md#update-the-conda-forge-recipe)
