ABOUT
====================

This module is a simple plugin for the dancer web framework. It easily enables you to add github authentication into your application. Additionally it adds two session objects (github_user and github_access_token) into your app.

USAGE 
====================

This is a sample of what your application should look like with this plugin

    package YourDancerApplication;

	  use Dancer ':syntax';
	  use Dancer::Plugin::Auth::Github;

	  #You must use a session backend. 
	  #You should be able to use any backend support by dancer.
	  set 'session'      => 'Simple';

	  #make sure you call this first.
	  #initializes the config
	  auth_github_init();

	  hook before => sub {
	      #we don't want to be in a redirect loop
	      return if request->path =~ m{/auth/github/callback};
	      if (not session('github_user')) {
	        redirect auth_github_authenticate_url;
	      }
	  };

	  #by default success will redirect to this route
	  get '/' => sub {
	      "Hello, ".session('github_user')->{'login'};
	      #For all the github_user properties
	      #look at http://developer.github.com/v3/users/
	      #See the Response for "Get the authenticated user"
	  };

	  #additionally the plugin adds session('github_access_token')
	  #so you can use it if you're doing other things with GitHub Api.

	  #by default authentication failure will redirect to this route
	  get '/auth/github/failed' => sub { return "Github authentication Failed" };


