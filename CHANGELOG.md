## Turbolinks (master)

*   Fix HTML5 autofocus for Firefox.

    *Nick Reed*
    
## Turbolinks 2.2.2 (April 6, 2014)

*   Always use absolute URLs as cache keys and in state objects.  Eliminates possibility of multiple 
    cache objects for the same page. 

    *Nick Reed*

*   Allow periods in a link's query string (ex. `/users?sort=account.email`) without it interfering with 
    the extension check.

    *Kuba Kuźma*

*   Don't process links with no `href` attribute.

    *Nick Reed*

## Turbolinks 2.2.1 (January 30, 2014)

*   Do not store redirect_to location in session if the request did not come from Turbolinks.  Fixes
    rare bug that manifests when directing items like image tag sources through ActionController and 
    subsequently redirecting those requests to a different origin. This would cause the session variable 
    to be set but never deleted (since the redirect request was external), resulting in the next request
    being declared a redirect no matter what. 

    *Nick Reed*

*   Extend handling of Firefox [history.state bug](https://bugzilla.mozilla.org/show_bug.cgi?id=949471) 
    through Firefox 27.

    *Nick Reed*
    
*   Fix handling of `:back` option in ActionView helpers `url_for` and `link_to`.

    *Nick Reed*

*   Delay binding of history change handler long enough to bypass the popstate event that some
    browsers fire on the initial page load.  Prevents duplicate requests in certain scenarios when
    traversing the browser history.

    *Anton Pawlik + Nick Reed*

## Turbolinks 2.2.0 (January 10, 2014)

*   Add transition cache feature. When enabled, cached copies of pages will be shown
    before fetching and displaying a new copy from the server. A individual page can be opted-out
    by adding `data-no-transition-cache` to any DOM element on the page.

    *Matt De Leon*

## Turbolinks 2.1.0 (December 17, 2013)

*   Improve browser support for `noscript` tag removal.

    *Nick Reed*

*   Add a `page:expire` event for garbage collecting purposes.  Triggered when a page is deleted
    from the cache due to reaching the cache size limit.  Pass the cached page object in with the
    event data. 

    *Nick Reed*

*   Allow configuration for additional link extensions to be processed by Turbolinks beyond `.html`.

    ```coffeescript
    Turbolinks.allowLinkExtensions()                # => ['html']
    Turbolinks.allowLinkExtensions 'md'             # => ['html', 'md']
    Turbolinks.allowLinkExtensions 'coffee', 'scss' # => ['html', 'md', 'coffee', 'scss']
    ```

    *Nick Reed*

*   Handle [bug](https://bugzilla.mozilla.org/show_bug.cgi?id=949471) in Firefox 26 where the initial
    value of history.state is undefined instead of null, which led to Turbolinks not being initialized.

    *Nick Reed*

## Turbolinks 2.0.0 (December 4, 2013) ##

*   Trigger page:update on page:change as well as jQuery's ajaxSuccess, if jQuery is available.
    This allows you to bind listeners to both full page changes and partial ajax updates.
    If you're not using jQuery, you can trigger page:update yourself on XMLHttpRequest to
    achieve the same result.
    
    *DHH*
    
*   Trigger page:change on DOMContentLoaded so you can bind initializers that need to be run 
    either on initial or subsequent page loads. This might be backwards incompatible, so we
    are also bumping the major version.
    
    *DHH*

*   Expire `request_method` cookie after reading it.

    *Nick Reed*

*   Trigger events on DOMContentLoaded and jQuery's ajaxSuccess regardless of whether Turbolinks
    is initialized or not.

    *Nick Reed*

## Turbolinks 1.3.1 (November 14, 2013) ##

*   Accommodate for bug in Chrome 31+ that causes `assetsChanged` to always return true. (#278)

    *Andrew Volozhanin + Nick Reed*
    
## Turbolinks 1.3.0 (July 11, 2013) ##

*   Change URL *after* fetching page.

    *Marek Labos*

*   Fix compatibility with `link_to :back`.

    *Marek Labos*

*   Send correct referer after asset change detected.

    *Marek Labos*
    
*   Add the `page:before-change` event, triggered when a Turbolinks-enabled link is clicked.
    Can be used to cancel the Turbolinks process.

    *Nick Reed*

*   Add Turbolinks.pagesCached() to the public API for getting and setting the size of the page cache.

    *Nick Reed*

## Turbolinks 1.2.0 (June 2, 2013) ##

*   Handle 5xx responses

    *Marek Labos*

*   Add the ability to not execute scripts on turbolinks page loads by
    specifying `data-turbolinks-eval=false` on the `<script>` tag. For example:
    `<script type="text/javascript" data-turbolinks-eval=false>`

    *Mario Visic*

*   Workaround for WebKit history state [bug](https://bugs.webkit.org/show_bug.cgi?id=93506) 
    with regards to the handling of 4xx responses.
    
    *Yasuharu Ozaki*
    
*   Extracted `fetchReplacement()` onload response-handling logic out into `validateResponse()`.

    *Nick Reed*

*   Changed response header name from `X-XHR-Current-Location` to `X-XHR-Redirected-To`.  The
    header will only be sent if there has been a redirect.  Fixes compatibility issue when 
    using `redirect_to` with options such as anchors or a trailing slash by storing the redirect
    location in a session variable and then using that value to set the response header.

    *Yasuharu Ozaki*

*   Escape URLs when checking for cross-origin redirects.

    *Nick Reed*

## Turbolinks 1.1.1 (April 3, 2013) ##

*   Improve performance of `constrainPageCacheTo`, `executeScriptTags`, and `removeNoscriptTags`
    by not gathering and returning the results of the loop comprehensions.
   
    *Tim Ruffles*

*   Change page without Turbolinks when Turbolinks.visit is called in Chrome on iOS

    *Frank Showalter*

*   Maintain the latest CSRF authenticity token in the `<meta name="csrf-token">` head tag, if it
    exists.
    
    *Nick Reed*
    
## Turbolinks 1.1.0 (March 24, 2013) ##

*   Added Turbolinks::XDomainBlocker module with after_filter to detect cross-domain
    redirects, returning 403 Forbidden so that the client will reissue the request
    without Turbolinks. (XSS Protection)

    *Mala*

*   Remove hash when checking for non-HTML links. (XSS Protection)

    *Nick Reed + Mala*

*   Check Content-Type response header.  Fall back to non-Turbolinks request unless the 
    header is either `text/html`, `application/xhtml+xml`, or `application/xml`. (XSS Protection)
    
    *Nick Reed + Mala*

*   Add a `page:receive` event, triggered the moment the ajax request returns, before any
    processing is done.
    
    *Ben Weintraub*
    
*   Explicitly set `useCapture` flag to default (false) on addEventListener and 
    removeEventListener calls. Removes errors on older browsers where the flag
    was required.
    
    *Matthieu Aussaguel*
    
*   Copy `<noscript>` tag list by slicing so that `removeNoscriptTags` works with
    multiple `<noscript>` tags.
    
    *Lion Vollnhals**

*   Copy `<script>` tag list by slicing so that `executeScriptTags` works in situations
    where a script removes itself from the DOM.
    
    *Nick Reed*

*   Add link to Turbolinks Compatibility project to README.

    *Nick Reed*
    
*   Reflect X-XHR-Referer in `request.referer`

    *Nick Reed*
    
*   Add `createDocumentUsingDOM` method to avoid DOMParser exceptions on certain
    browsers.
    
    *Nick Reed*
    
*   Add jquery.turbolinks note to README

    *Ry Walker*
    
*   Abort XHR on popstate / Maintain history when aborting XHR

    *Nick Reed*
    
## Turbolinks 1.0.0 (January 11, 2013) ##

*   Disable Turbolinks on Chrome for iOS

    *David Estes*

*   Disable Turbolinks after non-GET requests.  Adds a cookie named `request_method`.

    *Nick Reed*
    
*   Abort XHR request if the user clicks another link before it finishes.

    *David Estes*
    
*   Fall back to using `document.location.href = url` when there is an error so the
    applicationCache can be used in offline mode.
    
    *R. Potter*

*   Remove hash from XHR url to fix IE 10 bug.

    *Nick Reed*
    
*   Scroll to anchor link id if link has an anchor tag.

    *Yasuharu Ozaki*
    
*   Allow Turbolinks to function in non-Rails environments.

    *Yasuharu Ozaki*
    
*   Optimize script tag execution by using script injection instead of `window.eval`.
    Deprecates the use of the `data-turbolinks-evaluated` attribute.
    
    *Nick Reed + John-David Dalton*

## Turbolinks 0.6.1 (December 4, 2012) ##

*   Delay existing asset check until it's time to compare them to the fetched page's assets.

    *Nick Reed*

## Turbolinks 0.6.0 (December 4, 2012) ##

*   Only track assets that have a `data-turbolinks-track` attribute.

    *Yasuharu Ozaki*

*   Fix issue where anchors are being dropped from the URL when changing pages

    *Nick Reed*
    
*   Update `extractLink` to safeguard against links that are removed from the DOM 
    when clicked.
    
    *Gleb Mazovetskiy*
    
*   Improve `executeScriptTags` by handling case where `src` attribute is empty
    and using `window.eval` instead of `eval`.
    
    *itzki*
    
*   Add compatibility section to README

    *DHH*
    
*   Add support for executing external script tags in the body

    *Nick Reed*
    
*   Add note to README about dynamically added scripts

    *Manuel Meurer*
    
## Turbolinks 0.5.2 (November 26, 2012) ##

*   Prevent issue with ActionController::Live by moving `set_xhr_current_location`
    from an an after_filter to a before_filter.
    
    *Kentaro Kuribayashi*
    
*   Only execute script tags on XHR request load, not from history fetch.

    *David Estes*
    
*   Use `document.location.reload()` instead of `document.location.href = url` if the 
    assets have changed.
    
    *Yasuharu Ozaki*
    
*   Remove the `samePageLink` method.

    *Yasuharu Ozaki*
    
*   Change the `pageCache` from an array to a hash to fix back button handling when
    window.history.length is > 0 on page load.
    
    *Steven Bristol*

*   Handle case where a node, after being clicked, is removed from the DOM before `extractLink`
    can climb it's tree.
    
    *Steven Bristol*
    
*   Ignore links with a `target` attribute.

    *Steven Bristol*
    
*   Use timestamp to initialize history state position.

    *Yasuharo Ozaki*
    
*   Detect additional asset changes.

    *David Estes*

*   Add `coffee-rails` as a dependency.

    *Rafael Mendonça França*

## Turbolinks 0.5.1 (October 4, 2012) ##

*   Remember assets as soon as possible to prevent dynamic scripts from being checked
    and added.
    
    *David Estes*
    
*   Remove `cloneNode` since there is no need to reset the node's events.

    *David Estes*
    
*   Improve `createDocument` to only determine correct parser once.

    *David Estes*

## Turbolinks 0.5.0 (October 3, 2012) ##

*   Only execute script tags that contain Javascript.

    *Nick Reed*
    
*   Issue a full page load if the assets change.

    *DHH*

*   Detect asset changes.

    *David Estes*
    

#### Refer to the [commit history](commits/master) on GitHub to view changes prior to 0.5.0 ####
