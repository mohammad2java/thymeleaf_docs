# ThymeLeaf UserGuide

##Contents
		1) Installation with Spring boot
		2) basic concept
		3) comparison with freemarker
		4) expression and type
		5) decision control and loop control
		6) subTemplate concepts


----------------------------------------------------------
## 1) Installation with Spring boot
-------------------------------------------

		1) add thymeleaf dependency in pom.xml
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>

		2) configured spring.thymeleaf.prefix properties if template files is not in default 
		location.(classpath:/templates/)
		3) by-default cache is true ..for development you can set false using spring.thymeleaf.cache=false
		4) by-default suffix is .html ..you can set as you want using ..spring.thymeleaf.suffix=.htm



##2) basic concept
--------------------------
	it is based on attribute of html tag hence previously template(velocity,freemarkar) 
	does work based on special tag/template tag.Working concept is similer to angularjs framework.
	
	1) in freemarker
	------------------
	for assigning value to variable <#assign x = "plain">
	for displaying variable value- <p> ${x}  </p>
	 
	 2) in thymeleaf 
	 --------------------------
	 <div th:with="x=plain">
	 <p th:text="${x}"></p>
	 </div>
	 
	you can see thymeleaf does work based on attribute hence freemarker works based on tag.
	all html attribute will work along with thymeleaf prefix (default is th:)


##3) comparison with freemarker
---------------------------------
	-> Freemarker works based on special tag and special object
	-> Freemarker works based on object and its method like - obj.getName()
	-> Thymeleaf works based on special attribute of html tag.
	-> Thymeleaf works based on object and its property like -- obj.name
	


##4) expression and type			
--------------------------

	General Syntax of thymeleaf attribute
	-----------------------------------
	th:attribute_name="expression/value"
	example:
	th:text="#{message-key}"

	There are 5 type of expressions
	-----------------------------------
	1) Variable Expressions: ${...}
	2) Selection Variable Expressions: *{...}
	3) Message Expressions: #{...}
	4) Link URL Expressions: @{...}
	5) Fragment Expressions: ~{...}

	Literals
	------------
	1) Text literals: 'one text', 'Another one!',…
	2) Number literals: 0, 34, 3.0, 12.3,…
	3) Boolean literals: true, false
	4) Null literal: null
	5) Literal tokens: one, sometext, main,…

	Conditional operators:
	-----------------------------
	1) If-then: (if) ? (then)
	2) If-then-else: (if) ? (then) : (else)
	3) Default: (value) ?: (defaultvalue)



## Deep in Using Texts
----------------------

	GOOD TO KNOW 
	Diff bw th:text vs data-th:text
	---------------------------------
	th:text vs data-th:text both will work fine for template engine.
	but th:text is not valid for HTML5 specification it might raising complain for html5 document ...
	adding data- as prefix like data-th:text will be valid for HTML5 specification and also 
	work fine with Template engine.

	Escaped vs unescaped text
	----------------------------
	1) th:text ="#{home.msg}" ---it will escaped special character outcome might be changed 
	if 'home.msg' key having any special character like <> and all.
	2) th:utext ="#{home.msg}"  --it will display message without escaping of any present
	 special characters.
	100% outcome will be same as 'home.msg' key.mainly use if message having any html tags.
	
	
	
##Thymeleaf Basic Objects ( inbuild object with # prefix -${#locale.country}
-------------------------------------------------------------------------------
	1) #ctx: the context object.
	2) #vars: the context variables.
	3) #locale: the context locale.
	4) #request: (only in Web Contexts) the HttpServletRequest object.
	5) #response: (only in Web Contexts) the HttpServletResponse object.
	6) #session: (only in Web Contexts) the HttpSession object.
	7) #servletContext: (only in Web Contexts) the ServletContext object.
	
	Example:
	Established locale country: <span th:text="${#locale.country}">US</span>.
	
	
##Expression Utility Objects
----------------------------------

	1) #execInfo: information about the template being processed.
	2) #messages: methods for obtaining externalized messages inside variables expressions, 
	in the same way as they would be obtained 	using #{…} syntax.
	3) #uris: methods for escaping parts of URLs/URIs
	4) #conversions: methods for executing the configured conversion service (if any).
	5) #dates: methods for java.util.Date objects: formatting, component extraction, etc.
	6) #calendars: analogous to #dates, but for java.util.Calendar objects.
	7) #numbers: methods for formatting numeric objects.
	8) #strings: methods for String objects: contains, startsWith, prepending/appending, etc.
	9) #objects: methods for objects in general.
	10) #bools: methods for boolean evaluation.
	11) #arrays: methods for arrays.
	12) #lists: methods for lists.
	13) #sets: methods for sets.
	14) #maps: methods for maps.
	15) #aggregates: methods for creating aggregates on arrays or collections.
	16) #ids: methods for dealing with id attributes that might be repeated (for example, 
	as a result of an iteration).


Example:
-------
	${#messages.msg('msgKey')}

		${#execInfo.templateName}
		${#dates.format(date, 'dd/MMM/yyyy HH:mm')}
		${#calendars.format(today,'dd MMMM yyyy')}
		
		
##2) Selection Variable Expressions: *{...}
-------------------------------------------------
		
		
	<div th:object="${session.user}">
	    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
	    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
	    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
	 </div>
	 
##4) Link URL Expressions: @{...}
----------------------------------------
	 it will add prefix url (till application context) .  
		 <li><a href="product/list.html" th:href="@{/product/list}">Product List</a></li>





##decision control and loop control
======================================

decision condition
---------------------

		<a href="comments.html" 
         th:href="@{/product/comments(prodId=${prod.id})}" 
         th:if="${not #lists.isEmpty(prod.comments)}">view</a>


	Note: 1) th:if   just opposite is th:unless
-----------------------------------------------------------
			<a href="comments.html"
	   			th:href="@{/comments(prodId=${prod.id})}" 
	   		th:unless="${#lists.isEmpty(prod.comments)}">view</a>


switch condition:
--------------------


	<div th:switch="${user.role}">
	  <p th:case="'admin'">User is an administrator</p>
	  <p th:case="#{roles.manager}">User is a manager</p>
	  <p th:case="*">User is some other thing</p>
	</div>


Loop condition
---------------


	<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
    </tr>






##6) subTemplate concepts
-----------------------------------

	File name is footer.html and contents is -
	
	Defining fragments
	-----------------------
	
	<!DOCTYPE html>
	<html xmlns:th="http://www.thymeleaf.org">
	  <body>
	    <div th:fragment="copy">   //this is fragements/template code that can be resuable using th:insert or th:replace
	      &copy; 2011 The Good Thymes Virtual Grocery
	    </div>
	  </body>
	</html>
	
	
	
	
	Using/including fragments
	----------------------------
	
	General Syntax:
	------------------------
	th:insert/replace="~{templatename::selector}"
	
	example:
	
	<body>
	  ...
	  <div th:insert="~{footer :: copy}"></div>
	  
	</body>
		
		// footer (filename) && copy is fragments name(selector) of that file.
		
		
## Using ID as fragments of  template
-----------------------------------------------------------------------------------
	IN THIS CASE-SYNTAX-- th:insert/replace="~{templatename::#Id}"
	if there is not any fragements in templates then using Id attribute we can also include:
		 

	Example:
	----------
		definition....
		
		<div id="copy-section">
	  	&copy; 2011 The Good Thymes Virtual Grocery
		</div>
		
	implementation---
	-----------------------
			
	<body>
	<div th:insert="~{footer :: #copy-section}"></div>
	</body>
		
