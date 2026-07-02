<a id="user-guide-multi-column-index"></a>

# Multi-column operations

Skrub provides various tools to extend the use of single column transformers to
multiple columns.

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

* [Removing unneeded columns with `DropUninformative` and `Cleaner`](modules/multi_column_operations/drop_uninformativehtml.md)
* [Dropping columns with many missing values](modules/multi_column_operations/drop_uninformativehtml.md#dropping-columns-with-many-missing-values)
* [Applying `DropUninformative` only to a subset of columns](modules/multi_column_operations/drop_uninformativehtml.md#applying-dropuninformative-only-to-a-subset-of-columns)
* [Skrub Selectors, for selecting columns in a dataframe](modules/multi_column_operations/selectorshtml.md)
  * [Introduction to selectors](modules/multi_column_operations/selectorshtml.md#introduction-to-selectors)
  * [Type of selectors](modules/multi_column_operations/selectorshtml.md#type-of-selectors)
  * [Combining selectors](modules/multi_column_operations/selectorshtml.md#combining-selectors)
  * [Visualizing a selector](modules/multi_column_operations/selectorshtml.md#visualizing-a-selector)
  * [Using selectors with other skrub transformers](modules/multi_column_operations/selectorshtml.md#using-selectors-with-other-skrub-transformers)
* [Selecting based on dtype or data properties](modules/multi_column_operations/type_of_selectorshtml.md)
* [Categories of selectors](modules/multi_column_operations/type_of_selectorshtml.md#categories-of-selectors)
  * [Selectors based on column data types](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-data-types)
  * [Selectors based on column content and properties](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-content-and-properties)
  * [Selectors based on column names](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-names)
* [`filter()` and `filter_names()` to select with user-defined criteria](modules/multi_column_operations/advanced_selectorshtml.md)
  * [Example of custom criteria in `filter()`: selecting columns with outliers](modules/multi_column_operations/advanced_selectorshtml.md#example-of-custom-criteria-in-filter-selecting-columns-with-outliers)
* [Select columns with null values](modules/multi_column_operations/advanced_selectorshtml.md#select-columns-with-null-values)
  * [Example: Selecting columns by null percentage with `has_nulls()`](modules/multi_column_operations/advanced_selectorshtml.md#example-selecting-columns-by-null-percentage-with-has-nulls)
* [Detecting sessions in timestamped data with the SessionEncoder](modules/multi_column_operations/sessionizationhtml.md)
