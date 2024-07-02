# 40 Laravel Blade Directives and How to Create Custom 

## Conditional Directives
### 1. @if, @elseif, @else, and @endif

Use these directives for conditional statements.

@if($user->isAdmin())
    <p>Welcome, Admin!</p>
@elseif($user->isModerator())
    <p>Welcome, Moderator!</p>
@else
    <p>Welcome, User!</p>
@endif

2. @switch and @case

Use these directives for switch statements.

@switch($user->role)
    @case('admin')
        <p>Welcome, Admin!</p>
        @break
    @case('moderator')
        <p>Welcome, Moderator!</p>
        @break
    @default
        <p>Welcome, User!</p>
@endswitch

3. @isset and @endisset

Check if a variable is set.

@isset($records)
    <p>Records are available.</p>
@endisset

4. @empty and @endempty

Check if a variable is empty.

@empty($records)
    <p>No records found.</p>
@endempty

5. @auth and @endauth

Check if the user is authenticated.

Let’s say you have an admin guard for administrators. You can use the directives as follows:

@auth
    <p>Welcome, default user!</p>
@endauth

@auth('admin')
    <p>Welcome, Admin!</p>
@endauth

6. @guest and @endguest

Check if the user is a guest (unauthenticated). It can also be used specifically for a guard. @guest('guard')


@guest
    <p>You are not logged in as a regular user.</p>
@endguest

@guest('admin')
    <p>You are not logged in as an admin.</p>
@endguest

7. @can and @endcan

Check if the user has a specific ability.

@can('update', $post)
    <p>You can update this post.</p>
@endcan

8. @cannot and @endcannot

Check if the user does not have a specific ability. It is the inverse of @can .

@cannot('update', $post)
    <p>You cannot update this post.</p>
@endcannot

9. @env and @endenv

Check the current application environment.

@env('local')
    <p>Running in local environment.</p>
@endenv

@env(['staging', 'production'])
    // The application is running in "staging" or "production"...
@endenv

10. @production and @endproduction

Check if the environment is production.

@production
    <p>Running in production environment.</p>
@endproduction

Loop Directives
11. @forelse, and @endforelse

Loop through arrays or collections.

@foreach($items as $item)
    <p>{{ $item }}</p>
@endforeach

@foreach and @endforeach

these have the same function as @foreach, but here, you can specify what happens if the array is empty by using @ empty inside it.

@forelse($items as $item)
    <p>{{ $item }}</p>
@empty
    <p>No items found.</p>
@endforelse

12. @for and @endfor

Basic for loop.

@for($i = 0; $i < 10; $i++)
    <p>Iteration {{ $i }}</p>
@endfor

13. @while and @endwhile

Basic while loop.

@while($condition)
    <p>Condition is true.</p>
@endwhile

14. @loop — Loop Variables

Access loop properties.

@foreach($items as $item)
    @if($loop->first)
        <p>First iteration.</p>
    @endif
    @if($loop->last)
        <p>Last iteration.</p>
    @endif
    <p>{{ $item }}</p>
@endforeach

There are quite a number of loop variables. You can see the full list here
Laravel Loop Variables - The PHP Framework For Web Artisans
The loop variable contains a variety of useful properties…

laravel.com
Attribute Directives
15. @class

Conditionally add CSS classes.

<div @class(['alert', 'alert-danger' => $hasError])>
    <p>Content goes here.</p>
</div>

16. @disabled

Conditionally disable an element.

<input type="text" @disabled($isDisabled)>

17. @checked, @selected, and @readonly

Conditionally check a checkbox, select an option, and make a form read-only.

<input type="checkbox" @checked($isChecked)>
<option @selected($isSelected)>Option</option>
<option @selected($isSelected)>Option</option>

18. @prepend

If you would like to prepend content onto the beginning of a stack, you should use the @prepend directive:

@push('scripts')
    This will be second...
@endpush
 
// Later...
 
@prepend('scripts')
    This will be first...
@endprepend

Component and Layout Directives
19. @include, @includeIf, @includeWhen, and @includeUnless

These directives are used to include subviews within your Blade templates. They help in breaking down a large view into smaller, manageable pieces, making your code more modular and maintainable. Here’s a detailed look at each directive:
@include

The @include directive is used to include a Blade view. It’s the most basic way to insert a subview into your main view.

@include('partials.header')
@include('partials.header', ['title' => 'My Page Title']) // with data

@includeIf

The @includeIf directive includes a subview only if the specified view exists. This can prevent errors if the view might not be present.

@includeIf('folder.filename')
@includeIf('partials.header', ['title' => 'My Page Title']) // with data

@includeWhen

The @includeWhen directive conditionally includes a subview based on a given boolean expression. This allows you to include views dynamically.

@includeWhen($condition, 'partials.header')
@includeWhen($user->isAdmin(), 'partials.header', ['title' => 'Admin Page'])

@includeUnless

The @includeUnless directive includes a subview unless a given boolean expression evaluates to true. You can already guess that it’s the inverse of @includeWhen.

@includeUnless($condition, 'partials.header')
@includeUnless($user->isGuest(), 'partials.header', ['title' => 'Member Page'])

20. @extends and @endsection

Extend a layout. It works closely with @yield and @section.

@extends('layouts.app')

@section('content')
    <p>Content goes here.</p>
@endsection

21. @yield and @section

Define and yield sections.

// In layout file
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    @yield('content')
</body>
</html>

// In child view
@extends('layout')

@section('title', 'Page Title')

@section('content')
    <p>Page content goes here.</p>
@endsection

22. @push and @stack

Push and stack content.

// In layout file
<head>
    @stack('scripts')
</head>

// In child view
@push('scripts')
    <script src="script.js"></script>
@endpush

If you would like to @push content if a given boolean expression evaluates to true, you may use the @pushIf directive:

@push('scripts')
    <script src="/example.js"></script>
@endpush

23. @component and @endcomponent

Create and use components. This is particularly using in older codebases. It gives you better control over your components in

@component('components.alert', ['type' => 'danger'])
    <strong>Error!</strong> Something went wrong.
@endcomponent

24. @slot

Define slots in components.

@component('components.alert')
    @slot //unnamed
        Warning!
    @endslot

    @slot('title')
        Alert Title
    @endslot
    Alert message goes here.
@endcomponent

CSRF and Method Directives
25. @csrf

Include CSRF token in forms. It performs the same action as {{csrf_field()}} helper function. I explained this in my article about helper functions

<form method="POST" action="/profile">
    @csrf
    <!-- Form fields -->
</form>

40 Lesser-Known Laravel Helper Functions Every Laravel Developer Must Know, Plus How to Build Your…
While Laravel’s well-known helper functions are widely used, there are many lesser-known ones that can significantly…

medium.com
26. @method

Spoof HTTP methods. It generates a hidden input field containing the spoofed HTTP verb. This is essential for forms that need to submit a method other than GET or POST.

<form method="POST" action="/profile">
    @method('PUT')
    @csrf
    <!-- Form fields -->
</form>

Debugging and Utility Directives
27. @dd and @dump

The @dd directive stands for "dump and die." It outputs the value of the variable and immediately stops the execution of the script.

The @dump directive dumps the value of the variable to the browser without stopping the execution of the script.

@dd($user)
@dump($user)

28. @json

Encode data as JSON. It can come in handy.

<script>
    var data = @json($array);
</script>

29. @inject

The @inject directive in Blade is used to inject a service or a class instance into your view. This can be particularly useful for accessing services or repositories directly within your Blade templates without having to pass them from the controller.

@inject('userModel', 'App\Models\User')

<div>
    Total Users: {{ $userModel->count() }}
</div>

@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {{ $metrics->monthlyRevenue() }}
</div>

30. @verbatim and @endverbatim

Escape Blade syntax. If you are displaying JavaScript variables in a large portion of your template, you may wrap the HTML in the @verbatim directive so that you do not have to prefix each Blade echo statement with an @ symbol.

@verbatim
    <div>
        This will not be processed by Blade.
        @{{ name }}
    </div>
@endverbatim

31. @unless

It is like the inverse of @if. Except a condition is met, do something else.

@unless (Auth::check())
    You are not signed in.
@endunless

32. @hasSection

You may determine if a template inheritance section has content using the @hasSection directive.

@hasSection('navigation')
    <div class="pull-right">
        @yield('navigation')
    </div>
 
    <div class="clearfix"></div>
@endif

33. @sectionMissing

You may use the sectionMissing directive to determine if a section does not have content.

@sectionMissing('navigation')
    <div class="pull-right">
        @include('default-navigation')
    </div>
@endif

34. @section

The @session directive may be used to determine if a session value exists. If the session value exists, the template contents within the @session and @endsession directives will be evaluated. Within the @session directive's contents, you may echo the $value variable to display the session value:

@session('status')
    <div class="p-4 bg-green-100">
        {{ $value }}
    </div>
@endsession

35. @style

The @style directive may be used to conditionally add inline CSS styles to an HTML element:

@php
    $isActive = true;
@endphp
 
<span @style([
    'background-color: red',
    'font-weight: bold' => $isActive,
])></span>
 
<span style="background-color: red; font-weight: bold;"></span>

36. @each

The @each directive in Laravel Blade allows you to combine loops and includes into a single concise line for rendering repetitive views. It simplifies the process of iterating over a collection or array and rendering a view for each item.

Assume you have an array of jobs that you want to display using a Blade partial called job.blade.php:

@each('partials.job', $jobs, 'job')

    'partials.job' is the Blade view that will be rendered for each $job in the $jobs array.

    $jobs is the array containing the list of jobs.

    'job' is the variable name that will be available in the partials.job view to represent each individual job.

Inside partials/job.blade.php:

<div class="job">
    <h3>{{ $job->title }}</h3>
    <p>{{ $job->description }}</p>
</div>

37. @once

The @once directive in Laravel Blade is indeed a useful tool for ensuring that a specific piece of content is rendered only once.

Let’s say you have a component that you render multiple times within a loop, and you want to include a piece of JavaScript in the header, but it should only be included once, regardless of how many times the component is rendered.

@once
    @push('scripts')
        <script>
            console.log('This script should only appear once.');
        </script>
    @endpush
@endonce

    Since the @once directive is often used in conjunction with the @push or @prepend directives, the @pushOnce and @prependOnce directives are available for your convenience: — Laravel Docs

@pushOnce('scripts')
    <script>
        // Your custom JavaScript...
    </script>
@endPushOnce

38. @use

This is used to import a class. It is similar to @inject .

@use('App\Models\Flight')
//Add an alias to it
@use('App\Models\Flight', 'FlightModel')


<p>Name: {{ $FlightModel->count() }}</p>

39. @fragment

You may occasionally need to only return a portion of a Blade template within your HTTP response. Laravel Blade “fragments” allow you to do just that. To get started, place a portion of your Blade template within @fragment and @endfragment directives:

@fragment('user-list')
    <ul>
        @foreach ($users as $user)
            <li>{{ $user->name }}</li>
        @endforeach
    </ul>
@endfragment

Then, when rendering the view that utilizes this template, you may invoke the fragment method to specify that only the specified fragment should be included in the outgoing HTTP response:

return view('dashboard', ['users' => $users])->fragment('user-list');

40. @php

This is one I always make sure not to use, but it can come in handy sometimes. In some situations, it’s useful to embed PHP code into your views. You can use the Blade @php directive to execute a block of plain PHP within your template.

@php
    $counter = 1;
@endphp

<b> {{$counter}} </b>

Now that we have seen so many awesome Blade directives, let us see how we can create our own custom directives.

Laravel Blade allows you to define your own custom directives using the directive method. When the Blade compiler encounters the custom directive, it will call the provided callback with the expression that the directive contains.
Creating Custom Directives

Define custom Laravel Blade directives in your AppServiceProvider.

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Blade;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Blade::directive('datetime', function ($expression) {
            return "<?php echo with({$expression})->format('m/d/Y H:i'); ?>";
        });
    }
}

Now you can use this directive as @datetime .

@datetime($user->created_at)

Let’s do something sweeter.
Scenario: I want to create a laravel blade directive that I will use to check if the current route name matches a given parameter. It should also include support for wildcards.

Here’s how we can get it done. Inside the boot method in your AppServiceProvider ,

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Blade;

class AppServiceProvider extends ServiceProvider
{
        Blade::directive('routeis', function ($expression) {
            return "<?php if(fnmatch({$expression}, Route::currentRouteName())): ?>";
        });

        Blade::directive('endrouteis', function () {
            return '<?php endif; ?>';
        });
}

You can now use the @routeis and @endrouteis directives in your Blade templates:

@routeis('webshop.checkout')
    <p>This content is only visible on the checkout route.</p>
@endrouteis

@routeis('blog.post.*')
    <p>This content is visible on all blog post routes.</p>
@endrouteis

Finally…
Creating Custom If Statements

Programming a custom directive is sometimes more complex than necessary when defining simple, custom conditional statements. For that reason, Blade provides a Blade::if method which allows you to quickly define custom conditional directives using closures. For example, let's define a custom conditional that checks the configured default "disk" for the application. We may do this in the boot method of our AppServiceProvider:

    Some part was lifted from the official Laravel Documentation.

use Illuminate\Support\Facades\Blade;
 
/**
 * Bootstrap any application services.
 */
public function boot(): void
{
    Blade::if('disk', function (string $value) {
        return config('filesystems.default') === $value;
    });
}

Once the custom conditional has been defined, you can use it within your templates:

@disk('local')
    <!-- The application is using the local disk... -->
@elsedisk('s3')
    <!-- The application is using the s3 disk... -->
@else
    <!-- The application is using some other disk... -->
@enddisk
 
@unlessdisk('local')
    <!-- The application is not using the local disk... -->
@enddisk

