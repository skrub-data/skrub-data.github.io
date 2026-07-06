<a id="user-guide-encoders-index"></a>

# Column-level feature extraction

Skrub provides various transformers that help with feature engineering numeric,
datetime and categorical data. The encoders covered in this section convert the
raw features found in an input dataframe into numeric features that can be used
directly by machine learning models.

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

* [Encoding string and text columns as numeric features](modules/column_level_featurizing/feature_engineering_categoricalhtml.md)
  * [Choosing the right encoder for the job](modules/column_level_featurizing/feature_engineering_categoricalhtml.md#choosing-the-right-encoder-for-the-job)
* [Handling datetimes: parsing from strings and encoding as numbers](modules/column_level_featurizing/feature_engineering_datetimeshtml.md)
  * [Parsing Datetime Strings with `ToDatetime`](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#parsing-datetime-strings-with-todatetime)
    * [Dealing with Time zones](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#dealing-with-time-zones)
    * [Caveats when dealing with month first/day first conventions](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#caveats-when-dealing-with-month-first-day-first-conventions)
  * [Encoding and Feature Engineering with `DatetimeEncoder`](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#encoding-and-feature-engineering-with-datetimeencoder)
* [Parsing and scaling numeric features](modules/column_level_featurizing/feature_engineering_numericalhtml.md)
  * [Converting heterogeneous numeric values to uniform float32](modules/column_level_featurizing/feature_engineering_numericalhtml.md#converting-heterogeneous-numeric-values-to-uniform-float32)
  * [Robust scaling of numeric features using `SquashingScaler`](modules/column_level_featurizing/feature_engineering_numericalhtml.md#robust-scaling-of-numeric-features-using-squashingscaler)
* [Advanced columnwise operations](modules/column_level_featurizing/advanced_columnwise_operationshtml.md)
  * [The single column transformer](modules/column_level_featurizing/advanced_columnwise_operationshtml.md#the-single-column-transformer)
  * [Rejection handling with `ApplyToCols` and `core.RejectColumn`](modules/column_level_featurizing/advanced_columnwise_operationshtml.md#rejection-handling-with-applytocols-and-rejectcolumn)
