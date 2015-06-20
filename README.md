<p style="text-align: center;"><img class="aligncenter  wp-image-1321" alt="Article3_Medium" src="http://blog.incframework.com/upload/Article3_Medium.png" /></p>
<p style="text-align: left;"><strong>disclaimer</strong>: contrasting does not mean breaking out a “Holy War”, but presenting an overview of the tasks performed using one instrument as compared to the other. Available <a href="https://github.com/IncodingSoftware/inc-todo-refactor">source code</a>  and <a href="http://todo.incframework.com/">live demo</a> IML TODO ( like are MVC TODO ).<i>                                                                                                        </i></p>
What are we going to compare ?
<ul>
	<li>Controller</li>
	<li>Inheritance</li>
</ul>
<i>note: is missing in IML                                                                                                                                                      </i>
<ul>
	<li>Accessing server</li>
	<li>Push data</li>
	<li>Submit form</li>
	<li>Template</li>
	<li>Promises</li>
</ul>
<i>Note: is missing in IML and may only be useful when working with outside services, otherwise a thoughtful selection of data to retrieve is used to solve the tasks.                                                                                                                                 </i>

We have selected the options that make sense, since within the asp.net mvc frame we can see no use changing constants or localization support.

Note: the fact that the development is carried out within the asp.net mvc frame will be taken in to account.
<h3>The way we are going to compare ?</h3>
The methodology is very simple, here is the data sheet of AngularJS ( both View and Js ) and IML (View only ) variants with further reasoning why IML is better. We are going point out the advantages but will be glad to see all the IML drawbacks as well in the comments, so any of your considerations are welcome.

<em> </em>
<h1 style="text-align: left;">Controller</h1>
<div style="width: 45%; padding: 0 10pt 0 0; float: left;">
<h3 style="text-align: center;">Angular JS</h3>
<h5>View</h5>
<pre class="lang:c# decode:true">&lt;div ng-controller="angularController"&gt;
&lt;button ng-click="sayHello()"&gt;Say&lt;/button&gt;
&lt;/div&gt;</pre>
<h5>JS</h5>
<pre class="lang:c# decode:true">app.controller('angualrController', function ($scope) {
  $scope.sayHello = function(){
      alert('Hello')  
  } 
});</pre>
</div>
<div style="width: 55%; padding: 0 10pt 0 0; float: right;">
<h3 style="text-align: center;">IML</h3>
<pre class="lang:c# decode:true">@(Html.When(JqueryBind.Click)
      .Do()
      .Direct()
      .OnSuccess(dsl =&gt; dsl.Utilities.Window.Alert("Hello"))
      .AsHtmlAttributes()
      .ToButton("Say"))</pre>
</div>
<div style="clear: both;"></div>
<h5>Why is it better :</h5>
<ul>
	<li><strong>Behavior and marking together </strong></li>
</ul>
<em>Note : this may seem disputable to many, since logics complicates the webpage designer’s work, however C# code in the View ( cshtml ) is an integral part within the asp.net mvc frame, so the advantages that may be received compensate quite an abstract model of developing logics separately from marking.</em>
<ul>
	<li><strong> Initilesence support  </strong></li>
</ul>
<em>Note: AngularJs attributes are not standard html, therefore there will be no syntax highlighting or code completion,while IML is a C# library.</em>
<ul>
	<li><strong>Events represent sentinels</strong></li>
</ul>
<em>note: grouping is simplified when it is necessary to repeat the actions for another event When(JqueryBind.Click | JqueryBind.Focus) </em>
<ul>
	<li><strong>• Control over the event behavior ( Prevent Default, Stop Propagation) </strong></li>
</ul>
<em>Note: AngularJS allows controlling it within the controller method framework whereas IML includes it as an integral part of the layout. </em>
<h1 style="text-align: left;">Accessing Server</h1>
<h6>Accessing Server</h6>
<pre class="lang:c# decode:true">public ActionResult Get(GetProductByCodeQuery query)
{
 List  vms = dispatcher.Query(query);
 return IncJson(vms); // for AngularJs need use Json with AllowGet
}</pre>
<div style="width: 45%; padding: 0 10pt 0 0; float: left;">
<h5 style="text-align: center;">AngularJS</h5>
<h5>View</h5>
<pre class="lang:c# decode:true">&lt;div ng-&gt;
 @Html.TextBoxFor(r =&gt; r.Code)
 &lt;label&gt;{{model.Name}}&lt;/label&gt;
 &lt;button&gt;Get name&lt;/button&gt;
&lt;/div&gt;</pre>
<h4><span style="font-size: 0.83em;">JS</span></h4>
<pre class="lang:c# decode:true">kitchen.controller('productController', function ($scope, $http) {
  $scope.get = function(){
    $http.get({
                url:'product/get',
                params:{Code:$scope.Name}
              })
          .success(function(data) { 
              $scope.Name = data.Name
           });
 }
});</pre>
</div>
<div style="width: 55%; padding: 0 10pt 0 0; float: right;">
<h5 style="text-align: center;"> IML</h5>
<pre class="lang:c# decode:true">@{ var labelId = Guid.NewGuid().ToString(); }
@Html.TextBoxFor(r=&gt;r.Code)
@(Html.When(JqueryBind.Click)
  .Do()
  .AjaxGet(Url.Action("Product","Get",new {
                       Code = Html.Selector().Name(r=&gt;r.Code)
                      })
  .OnSuccess(dsl =&gt; dsl.WithId(labelId).Core().Insert.For(r=&gt;r.Name).Text())
  .AsHtmlAttributes()
  .ToButton("Get name"))</pre>
<em>Note: when constructing the url, one may use as Routes not only anonymous object but also typed variant Url.Action(“Product”,”Get”,new GetProductQuery() { Code = Html.Selector().Name(r=&gt;r.Code) }) </em>

</div>
<div style="clear: both;"></div>
<h5 style="text-align: left;">Why is it better:</h5>
<ul>
	<li><strong>Typing at all stages</strong></li>
</ul>
<ul>
<ul>
	<li><strong>Url</strong> - address in AngularJs is built without server compound, which prevents from taking route into consideration or to pass into Action from the code.</li>
</ul>
</ul>
<em>Note: one can’t use server variables under js code, since AngularJs manual recommends to put JavaScript code into a separate file. </em>
<ul>
<ul>
	<li><strong>Query</strong> - AngularJs is not connected to the server model and does not let to get the scheme model which , similarly to the Url, doesn’t allow using tools to rename or go to declaration.</li>
</ul>
</ul>
<em>Note: In case of IML, if we rename the member variable Name or Code as GetProductQuery changes will affect the client part as well, for AngularJs however<strong> {{model.Name}}</strong> and <strong>$scope.Name</strong> shall also be changed for new values. </em>
<ul>
<ul>
	<li><strong>Selector</strong> - to indicate the inquiry parameters within IML one may use to use the powerful tool to obtain values from Dom (hash, cookies, js variable and etc) objects</li>
</ul>
</ul>
<ul>
	<li><strong>MVD </strong></li>
</ul>
In<strong> Incoding Framework</strong> one may do without writing supplementary Controller and Action when using <a title="MVD ( Model View Dispatcher )" href="http://blog.incframework.com/model-view-dispatcher/"><strong>MVD</strong></a>
<pre class="lang:c# decode:true">Url.Dispatcher().Query(new GetProductQuery() {Code = Html.Selector().Name(r=&gt;r.Code)}).AsJson()</pre>
<h1 style="text-align: left;">Push data</h1>
<h6>Server code</h6>
<pre class="lang:c# decode:true">public ActionResult Add(AddCommand input)
{ 
dispatcher.Push(input);
return IncJson(); // for AngularJS need use Json()
}</pre>
<em> Note: the server part is the same for both AngularJS and IML </em>
<div style="width: 45%; padding: 0 10pt 0 0; float: left;">
<h5 style="text-align: center;">Angular</h5>
<h5>View</h5>
<pre class="lang:c# decode:true">&lt;div ng-controller="productController"&gt;
  @Html.TextBoxFor(r =&gt; r.Name) 
  @Html.CheckboxFor(r =&gt; r.IsGood)
  &lt;button ng-click="add"&gt; Add &lt;/button&gt;
&lt;/div&gt;</pre>
<span style="font-size: 0.83em;"> JS</span>
<pre class="lang:c# decode:true">kitchen.controller('productController', function ($scope, $http) {
 $scope.add = function(){ 
   $http({
       url: 'product/Add',
       method: "POST",
       data: { 
             Name : $scope.Name,
             IsGood : $scope.IsGood
             }
         })
      .success(function(data) { alert('success') });
});</pre>
</div>
<div style="width: 55%; padding: 0 10pt 0 0; float: right;">
<h5 style="text-align: center;">IML</h5>
<pre class="lang:c# decode:true">@Html.TextBoxFor(r=&gt;r.Name)
@Html.CheckboxFor(r=&gt;r.IsGood)
@(Html.When(JqueryBind.Click)
      .Do()
      .AjaxPost(Url.Action("Product","Add",new {
                                                 Name = Html.Selector().Name(r=&gt;r.Name),
                                                 IsGood = Html.Selector().Name(r=&gt;r.IsGood)
                                               }))
      .OnSuccess(dsl =&gt; dsl.Utilities.Window.Alert("Success"))
      .AsHtmlAttributes()
      .ToButton("Add"))</pre>
</div>
<div style="clear: both;"></div>
<h5>Why is it better:</h5>
<ul>
	<li><strong> MVD </strong></li>
</ul>
<p style="text-align: left;">As in the previous example when we were making Query, in Incoding Framework pushmay be performed without writing Action.</p>

<pre class="lang:c# decode:true">Url.Disaptcher().Push&lt;AddProductCommand&gt;( new {
                                   Name = Html.Selector().Name(r=&gt;r.Name),
                                   IsGood = Html.Selector().Name(r=&gt;r.IsGood)
                            })</pre>
<ul>
	<li><strong> Selector abstracts from a way of obtaining value</strong></li>
</ul>
<h2 style="text-align: left;"></h2>
<h2 style="text-align: left;">Submit Form</h2>
<div style="width: 45%; padding: 0 10pt 0 0; float: left;">
<h5 style="text-align: center;">Angular</h5>
<h5>View</h5>
<pre class="lang:c# decode:true">&lt;div ng-controller="productController"&gt; 
  &lt;form name="AddForm"&gt;
    @Html.TextBoxFor(r =&gt; r.Name)
    @Html.CheckboxFor(r =&gt; r.IsGood) 
    &lt;input type="submit" value="save"  ng-submit="submit" /&gt;
  &lt;/form&gt;
&lt;/div&gt;</pre>
<span style="font-size: 0.83em;">JS</span>
<pre class="lang:c# decode:true">controller('productController', function ($scope, $http) {
 $scope.submit = function(){ 
   $http({
       url: 'product/Add',
       method: "POST",
       data: angular.toJson($scope.addForm)
        }).success(function(data) { alert('success') });
});</pre>
</div>
<div style="width: 55%; padding: 0 10pt 0 0; float: right;">
<h5 style="text-align: center;"> IML</h5>
<pre class="lang:c# decode:true">@using(Html.When(JqueryBind.Submit)
           .DoWithPreventDefault()
           .Submit()
           .OnSuccess(dsl =&gt; dsl.Utilities.Window.Alert("Success"))
           .AsHtmlAttributes()
           .ToBeginForm(Url.Action("Product","Add")))
 {
    @Html.TextBoxFor(r=&gt;r.Name)
    @Html.CheckboxFor(r=&gt;r.IsGood)
    &lt;input type="submit" value="add"&gt;
 }</pre>
</div>
<div style="clear: both;"></div>
<h5 style="text-align: left;">Why is it better:</h5>
<ul>
	<li><strong>Form sending in one line </strong></li>
</ul>
<em>note: angularJs works with the form in the same way as with an ordinary ajax query, which demands stating Url, Type, Data parameters.</em>
<ul>
	<li><strong>Html Helper </strong></li>
</ul>
<pre class="lang:c# decode:true"> @using (Html.Todo().BeginForm&lt;AddTodoCommand&gt;()
 {
  @Html.TextBoxFor(r =&gt; r.Title, new { placeholder = "What needs to be done?", autofocus = "" })
 }</pre>
<em> note: for understand power full Html helper need compare <a href="https://github.com/IncodingSoftware/inc-todo">inc-todo</a> and <a href="https://github.com/IncodingSoftware/inc-todo-refactor">inc-refactor-todo</a></em>

&nbsp;
<h1 style="text-align: left;">Template</h1>
<h6>Server code</h6>
<pre class="lang:c# decode:true">public ActionResult Fetch()
{
   var vms = new List()
     {
      new PersonVm(){ Last= "Vasy", First = "Smith", Middle = "Junior"},
      new PersonVm(){ Last= "Vlad", First = "Smith", Middle = "Mr"},
     };
  return IncJson(vms); // for angular need use Json with AllowGet
}</pre>
<em> Note: server part is the same both for AngularJS and IML. </em>
<div style="width: 45%; padding: 0 10pt 0 0; float: left;">
<h5 style="text-align: center;">Angular JS</h5>
<h5>Template</h5>
<pre class="lang:c# decode:true ">&lt;script id="person.html" type="text/ng-template"&gt;
{{person.last}}, {{person.first}} {{person.middle}}
&lt;/script&gt;</pre>
<h5><span style="font-size: 0.83em;">View</span></h5>
<div>
<pre class="lang:c# decode:true">&lt;div ng-controller="productAddForm"&gt;
  &lt;div ng-repear="person in persons" ng-template="person.html"&gt;   
  &lt;/div&gt;
&lt;/div&gt;</pre>
</div>
<h5>JS</h5>
<pre class="lang:c# decode:true">app.controller('personController', function ($scope,$http) {
   $scope.refresh= function(){
      $http.get('person/fetch', function(data){ 
                          $scope.Persons= data 
      });
   } 
});</pre>
</div>
<div style="width: 55%; padding: 0 10pt 0 0; float: right;">
<h5 style="text-align: center;">IML</h5>
<h5>Template</h5>
<pre class="lang:c# decode:true">@{
    var tmplId = Guid.NewGuid().ToString();
    using (var template = Html.Incoding().Template(tmplId))
    {
      using (var each = template.ForEach())
      {  @each.For(r=&gt;r.First),@each.For(r=&gt;r.Middle),@each.For(r=&gt;r.Last) }
    }
}</pre>
<h5><span style="font-size: 0.83em;">View</span></h5>
<pre class="lang:c# decode:true">@(Html.When(JqueryBind.InitIncoding)
 .Do()
 .AjaxGet(Url.Action("Personal","Fetch"))
 .OnSuccess(dsl =&gt; dsl.Self().Core().Insert.WithTemplateById(tmplId).Html())
 .AsHtmlAttributes()
 .ToDiv())</pre>
</div>
<div style="clear: both;"></div>
<h5>Why is it better :</h5>
<ul>
	<li><strong>Typing again </strong></li>
</ul>
<em>Note: Incoding template demands more coding for typed syntax implementation but this is compensated with further support.</em>
<ul>
	<li><strong>•One template for a single or multiple objects.</strong></li>
	<li><strong>Template engine replacement.</strong></li>
	<li><strong>Template search on Selector, which has more options ( ajax , cookies and etc )</strong></li>
	<li><strong>Cache ( <a title="Клиентские template" href="http://blog.incframework.com/client-template/">more</a> )</strong></li>
</ul>
AngularJs has its own mechanism of Cache processing, but there are significant differences.
<ul>
<ul>
	<li><strong>Each template has to be fixed (adapted)separately</strong></li>
</ul>
</ul>
<pre class="lang:c# decode:true">myApp.run(function($templateCache) {
  $templateCache.put('templateId.html', 'This is the content of the template');
});</pre>
<ul>
<ul>
	<li><strong>Isn’t stored from session to session (isn’t stored between sessions)</strong></li>
</ul>
</ul>
<em>Note : Incoding Framework stores template pre-compiled version in local storage, thus allowing the user to address directly to the local storage after pre-compiling. </em>
<h4>So, what are the common advantages:</h4>
<ul>
	<li><strong>es, it’s typing again - </strong>achieved by using C# at all development stages ( template , client scenario and etc )</li>
	<li><strong>One language both for backend and frontend - </strong>developer skillful in C# can easily master IML and subsequently fetch Command and Query on the client.</li>
</ul>
<p style="text-align: left;"><strong>conclusion</strong>: AngularJs expands mvc architecture on the client, thus allowing to structure the code on one hand, but on the other hand causes more support problems. asp.net mvc developer has a server mvc implementation, where complications can be avoided due to the use of more powerful and suitable languages.</p>
