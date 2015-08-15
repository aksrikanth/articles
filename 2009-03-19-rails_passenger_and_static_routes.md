<!-- Thu 19 Mar 2009 -->

I've just updated rails to the newly released 2.3.2, and deployed it using passenger (mod_rails) 2.1.2.
However there was a problem with the routing of static files.
I use static [haml](http://haml.hamptoncatlin.com/) files to serve the static content on the site like my
[code](/code) and [publications](/publications) pages.

Initially I was just using a regular resource route to serve static content:

    map.resources :static, :controller => 'static_files'

This worked with mongrel while developing, but it didn't work with passenger and apache.
Passing numerical parameters to the static controller worked, but string arguments got 404 responses from apache.
I guess passenger registers paths with numeric ids by default.
The next thing I tried was using a named path:

    map.static 'static/*path', :controller => 'static_files', :action => 'show'

This failed to work entirely.
I haven't managed to figure out why, but the `params[:path]` variable was not set correctly or something.

Finally I found a [blog post](http://snafu.diarrhea.ch/blog/article/4-serving-static-content-with-rails) that detailed an approach that worked.
The route looked like:

    map.connect '*path', :controller => 'static_files'

That seemed to work, but I had to manually type out the full path for each static file, and that seemed tedious.
One of the bigger advantages of routes in rails is that you can use routing helpers like `code_path` without having to resort to string urls.

I solved the problem by adding some paths to routes.rb:

    map.connect '*path', :controller => 'static_files'
    map.code '/code', :controller => 'static_files'
    map.publications '/publications', :controller => 'static_files'

These routes are never used for route resolution since the `*path` rule is more general and appears first.
However it does generate the routing helpers and I can live with that hack for now.
