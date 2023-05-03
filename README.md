Download Link: https://assignmentchef.com/product/solved-cs0449-lab-5-advanced-pointers
<br>



Pointers are super important when writing low-level programs! GET COMFORTABLE WITH EM!

In this lab, you’ll be writing a <strong>generic array-filtering function.</strong> This will make use of pointer arithmetic, function pointers, and pointer casting. This is an actually useful function! Filtering values out of an array is a very common operation.

<h2>Starting off</h2>

<a href="/teaching/classes/cs0449/labs/lab5.c">Get the starting file here.</a> Rename it properly, and upload to thoth.

Open it and read the comments. All of them.

<h2>Predicates</h2>

“Predicate” is a common programming term which means “something that gives a yes-or-no answer.”

The filter function’s predicate must be a function which:

<ul>

 <li>takes a const void* which points to <strong>a value from the array</strong></li>

 <li>returns an integer:

  <ul>

   <li>0 for <strong>false</strong> (ignore the item)</li>

   <li>nonzero for <strong>true</strong> (put the item in the output array)

    <ul>

     <li>(This is common in C, because it didn’t use to have bool.)</li>

    </ul></li>

  </ul></li>

</ul>

<h2>Writing the predicate function</h2>

The less_than_50 function should <em>interpret its parameter as a pointer to a </em><em>float</em><em>,</em> and as the name implies, return a “true” (nonzero) value if it is less than 50.

Since the parameter is a const void*, you’ll have to cast the parameter to a different pointer type.

Have a look at <a href="/teaching/classes/cs0449/examples/14_qsort.c">how I wrote the comparison function in the qsort.c example</a> to get an idea of how to write this.

Hint: in C, comparison operators give an integer value. They give 1 if they’re true, and 0 if they’re false.

<h2>Writing the filter function</h2>

You didn’t read the comments, did you. &#x1f624;

Have a look at the code in main:

float filtered[NUM_VALUES];int filtered_len = filter(filtered, float_values, NUM_VALUES, sizeof(float), &amp;less_than_50); printf(“there are %d numbers less than 50:
”, filtered_len); for(int i = 0; i &lt; filtered_len; i++)    printf(“t%.2f
”, filtered[i]);

Look at the float_values array and think about what the output <em>should</em> look like. (There are 6 numbers less than 50, right?)

The filter function should work like this:

<ul>

 <li>for each item in the input array:

  <ul>

   <li>call the pred function with a pointer to that element</li>

   <li>if it returned “true”:

    <ul>

     <li>use memcpy to copy that item from the input array to the output array (see below)</li>

    </ul></li>

  </ul></li>

</ul>

In addition it should:

<ul>

 <li>keep a count of how many items “passed the test” (predicate returned “true”)</li>

 <li>return that count</li>

</ul>

<h3>memcpy</h3>

memset is used to fill in a blob of bytes with a value. memcpy is used to copy blobs of bytes from one place to another. It’s a very common function.

memcpy(dest, src, length);

This will copy length bytes from the memory pointed to by src into the memory pointed to by dest.

<h3>“Walking pointers”</h3>

You’re used to using [] to access values from arrays. But you can’t use [] on a void*. Instead, an easier technique is to use a “walking pointer.”

Instead of keeping a pointer to the beginning of an arrays, we <strong>move the pointer along, item by item, to access the array.</strong> Like this.

But there’s a catch: <strong>you can’t do pointer arithmetic on void pointers either!!</strong>

So if you want to move a void* over by <em>n</em> bytes, you have to:

<ul>

 <li>cast it to a char*</li>

 <li>add <em>n</em> to that</li>

 <li>store it back into the void*</li>

</ul>

All this can be done on <strong>one line.</strong> Don’t overcomplicate things.

<h3>Good luck, but some likely mistakes:</h3>

If you <strong>don’t move the pointer along the </strong><strong>input</strong><strong> array,</strong> you’ll get something like:

there are 10 numbers less than 50:        31.94        31.94        31.94        …etc…

If you <strong>don’t move the pointers by the right number of bytes,</strong> you might get something like:

there are 6 numbers less than 50:        127.76        36.10        0.00        …etc…

If you moved the input pointer right, but <strong>forgot to move the </strong><strong>output</strong><strong> pointer along:</strong>

there are 6 numbers less than 50:        19.60        0.00        …etc…

If you didn’t <strong>count properly,</strong> or maybe you didn’t <strong>respond to the predicate properly:</strong>

there are 0 numbers less than 50: