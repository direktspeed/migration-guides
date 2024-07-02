Migration Guides
A Collection of Migration Guides for React, Vue, jQuery, and DoneJS.

## Basic Facts
- The Browser DOM is extremely fast.
- There is a significant knowledge gap between those who create browsers and internet standards and so-called senior engineers.
- Fewer function calls and less frequent changes to abstractions over APIs designed to last, such as the Web API itself, lead to greater simplicity and maintainability.
- Modern browsers already offer a comprehensive set of functions that were previously provided by frameworks.
- The Mutation Observer API is not well documented in terms of its high-level functionality and how it fundamentally shifts web coding from reactive to imperative paradigms.

## Why This Guide?
Many beginners get lost when learning to code, often relying on frameworks that incur technical debt. They cannot judge the frameworks' implementation due to a lack of understanding. As one of the original developers of jQuery and many other frameworks, I aim to document a way out of dependency hell. This guide provides the standards for web development for 2030 and beyond, showing you how to adopt them incrementally today to be prepared for the future.

## Historical Fun Facts for Entertainment and Education
You might wonder why internet groups made certain decisions. The last 25 years of the internet have been a whirlwind. The number of coders has doubled every few years, but the number of senior developers has not scaled in the same way. Today, someone who can code a webpage might call themselves a senior developer simply because, within their group, they are relatively more experienced.

True senior developers are rare. While there are many experienced individuals, few can keep up with the rapid pace of change in technology.

## Conclusion
I have spent considerable time understanding why people argue as they do and how social dynamics influence these discussions. This set of guides aims to provide useful information for the future, ensuring that the knowledge is accessible and understandable.


# Browser Rendering Pipeline

1. Initial Parse Start
2. Head apply Head
3. Paint Head
4. Start Body parsing (Note: This can become complex with different content types like XHTML, XML, etc., but we are focusing on HTML now)
5. Second iteration of the JS stack and the first iteration of the module stack
6. First chunk mutation applied
7. First paint event, also known as `DOMContentLoaded`, which can race with the module stack if it does not use top-level await to block.

By design, the end of the first module stack iteration coincides with the content's first paint. Thus, a module can assume the DOM content is loaded and even painted.

Now, we can use the Mutation Observer to handle the page imperatively. Service workers could even modify the initial load from this point onward. It's important to note that service workers are registered for a scope and are used by an HTML document, not by other URLs accessed outside the scope of the HTML document that loads them. The scope only defines the subset of page requests the service worker should handle, so it's feasible to have different workers per page, not by path.

## How the Custom Element API Was Integrated and Polyfilled

Custom Elements allow you to insert elements into the code that get rendered directly as defined by previously registered handlers in the browser. The Mutation Observer API enables this functionality by running after the script loops but before the paint loops, where the animation frame runs after the paint loop to prepare the next paint loop. An infinite loop in the animation frame or the Mutation Observer blocks page rendering, just as an infinite loop in the main script loop would.

In the pre-Mutation Observer era (e.g., jQuery), we executed code in the main script loop, constantly checking for changes and manipulating internals to get events about those changes. This is no longer necessary. Today, the Mutation Observer loop and API have replaced these methods. Regardless of the framework or API used to change the DOM, the Mutation Observer detects the change before it is painted on the page. This allows for escaping dependency hell and performing incremental refactoring. If you have a highly encapsulated app, you can modularize it again at this point.



