---
title: Sidebars and other link macros
slug: MDN/Writing_guidelines/Page_structures/Quicklinks
page-type: mdn-writing-guide
---

{{MDNSidebar}}

MDN supports adding links and lists of links in sidebars and within the main content of a page. You can and often **should** use macros to generate links.
This article describes how to use the different MDN macros, demonstrating how to include sidebars to MDN pages and add always-up-to-date and maintained links within your content. Quickly.

In this guide, you will learn how to create a sidebar of links by just including a macro and how to add how to use those same macros to create sidebars with additional text. You will also learn about cross-reference macros to add links to other pages and macros to add lists of links to pages and subpages.

## Creating sidebars

Every page has a sidebar. These sidebars are created by Quicklinks. **Quicklinks** are MDN macros that create boxes containing a potentially hierarchical list of links to other pages on this site. They are mainly used in sidebars, though can also be used in the main content area.

> **Note:** The "Quicklinks" names comes from the ID used in server-side scripts to identify these content areas and may come from a shortening of [`id="Quick_links"`](#sidebars_adding_additional_content).

When a Quicklink is included, the server creates a section of content containing an unordered list of links. The links created, where they are displayed, and how they are displayed depend on the macro used and the parameters included in the markdown macro call. Some quicklinks include links based on a directory's structure or page type. Others include a list of predefined pages that are hard-coded in Yari.

### Fully macro-generated sidebars

To include only the content generated by the Quicklink macro, the macro is added immediately after the frontmatter and before the content on every page. For example, the first lines of this document are written as:

```md
---
title: Sidebars and other link macros
slug: MDN/Writing_guidelines/Page_structures/Quicklinks
page-type: mdn-writing-guide
---

\{{MDNSidebar}}
```

The `\{{MDNSidebar}}` is a Quicklink macro that adds the MDN sidebar to the page. Because we only want the content from the macro, and no additional content in our sidebar, the macro is placed immediately after the frontmatter.

Here are a few other Quicklink macros, with what they do:

- `\{{CSSRef}}`

  - : Present on every CSS page, it generates a CSS sidebar that includes links for modules, properties, selectors, combinators, pseudo-classes, pseudo-elements, at-rules, functions, and types, with all the link lists collapsed except for the link list for the current page type.

- `\{{DefaultAPISidebar("<API_Title>")}}`

  - : The API sidebar displayed for overview pages; the single parameter is the name of the API group in GroupData.

- Other sidebar "quicklink" macros
  - : Other sidebar generating macro calls include `\{{GlossarySidebar}}`, `\{{LearnSidebar}}`,`\{{HTMLSidebar}}`, `\{{HTTPSidebar}}`, `\{{PWASidebar}}`, etc.

The appropriate macro to use depends on the page type and is listed in the template for each page type.

### Sidebars: adding additional content

Each of the Quicklink macros listed above creates a predefined sidebar structure.
If you need to append links before or after the sidebar structure generated by the macro, you can do so by:

1. Remove any sidebar Quicklink macro appearing immediately after the frontmatter and before the content, as each document can only have one sidebar.
2. At the end of the markdown file, add an HTML {{htmlelement("section")}} element and add `id="Quick_links"` (Hence the name "Quicklinks") to the opening tag.
3. Nest both the macro and any additional links within this `<section>`.

For example, adding the following at the end of a markdown file (and removing any sidebar macro from below the frontmatter), will create a sidebar containing the links to all the ARIA role pages, preceded by a link to the ARIA roles overview page:

```md
<section id="Quick_links">

1. [**WAI-ARIA roles**](/en-US/docs/Web/Accessibility/ARIA/Roles)

   \{{ListSubpagesForSidebar("/en-US/docs/Web/Accessibility/ARIA/Roles", "true")}}

</section>
```

> **Note:** This `<section>` must be appended to the end of the page, instead of between the frontmatter and the page content.

You can also fully craft a sidebar with more than one Quicklinks macro, without including a Quicklinks macro, with cross reference macros, or any combination of macros and markdown. The steps to creating a sidebar without a Quicklinks macro are similar to the steps for the ARIA example above:

1. Remove any sidebar Quicklink macro appearing immediately after the frontmatter and before the content.
2. At the end of the markdown file, add the following HTML markup as the last content of the markdown file: `<section id="Quick_links"></section>`.
3. Add an unordered list of links written in markdown between the opening and closing `<section>` tags.

```md
<section id="Quick_links">

- [WAI-ARIA `button` role](/en-US/docs/Web/Accessibility/ARIA/Roles/Button_role)
- \{{HTMLElement("button")}}
- \{{HTMLElement("input/button", "button")}} \{{HTMLElement("input")}}

</section>
```

> **Note:** To include content in your sidebar along with a quicklink macro, the extra content and the quicklinks macro must be placed at the end of the document. Only one sidebar is created per page, so any macro listed after the frontmatter must be removed.

## Main content area links

Macros are not limited to sidebars or even to long lists of links.

### Lists of links

Quicklink macros can also be used to add lists of links to the main area of the page. The following macros do not create a sidebar. They create a list of links. They can be included in the content of the page or within a sidebar.

- `\{{LandingPageListSubPages()}}`

  - : Inserts a definition list ({{HTMLelement("dl")}}) of the subpages of the current page, with each page's title as the {{HTMLelement("dt")}} term and its SEO summary as the {{HTMLelement("dd")}} term. The optional parameter accepts the slug or the parent page of the directory of pages to output instead of the subpages of the current page.

- `\{{ListSubpagesForSidebar(<parameters>)}}`

  - : Inserts a tree of subpages of the slug of the pages specified as the first parameter. Include two parameters, with the second being `true` or `1`, to display the links as plain text instead of like code. Include a third parameter, set to `true` or `1`, to include the parent page at the top of the list with the link text "Overview".

To add include either list of links as part of a sidebar, include the macro as described in [Sidebars: adding additional content](#sidebars-adding-additional-content).

### Cross-reference links

In addition to macros that create lists of links, there are macros that quickly create a single link to cross-reference a CSS, JavaScript, SVG, or HTML feature, including attributes, elements, properties, data types, and APIs. The macros that create single links require at least one parameter: the feature being referenced.

Some of these macros include (see [all the macros on Github](https://github.com/mdn/yari/tree/main/kumascript/macros)):

- [`\{{CSSxRef("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/cssxref.ejs)
- [`\{{DOMxRef("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/DOMxRef.ejs)
- [`\{{HTMLElement("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/HTMLElement.ejs)
- [`\{{glossary("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/Glossary.ejs)
- [`\{{JSxRef("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/jsxref.ejs)
- [`\{{SVGAttr("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/SVGAttr.ejs)
- [`\{{SVGElement("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/SVGElement.ejs)
- [`\{{HTTPMethod("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/HTTPMethod.ejs)
- [`\{{HTTPStatus("")}}`](https://github.com/mdn/yari/blob/main/kumascript/macros/HTTPStatus.ejs)

All the macros accept additional parameters. By default, the text displayed is the linked resource as written in the first parameter. The second parameter, if present, provides the link text. In most cases, these links are displayed like code, in a monospace font. To prevent HTML code semantics and CSS code styling, some of the cross-reference macros include a parameter with the value of `"nocode"` to disable this styling.

For example, `\{{CSSxRef("background-color")}}` creates the code link "{{CSSxRef("background-color")}}" and `{{domxref("CSS.supports_static", "check support", "nocode")}}` creates the plain text link "{{domxref("CSS.supports_static", "check support", "nocode")}}".

To learn which parameters each macro supports along with the order of parameters for each macro, the macro's source file, linked above, includes documentation. There is a [list of commonly used macros](/en-US/docs/MDN/Writing_guidelines/Page_structures/Macros/Commonly_used_macros), each of which outputs links in the main content area of the page.

## Underlying code for MDN macros

There are around 100 [available macros](https://github.com/mdn/yari/tree/main/kumascript/macros). Here are a few standard macros for generating quicklinks and an example of a hardcoded Yari macro list.

- [`CSSRef`](https://github.com/mdn/yari/blob/main/kumascript/macros/CSSRef.ejs)
  - : Builds the standard quicklinks for CSS Reference pages.
- [`HTMLSidebar`](https://github.com/mdn/yari/blob/main/kumascript/macros/HTMLSidebar.ejs)
  - : Builds the standard quicklinks for HTML Reference pages.
- [`QuickLinksWithSubpages`](https://github.com/mdn/yari/blob/main/kumascript/macros/QuickLinksWithSubpages.ejs)
  - : Creates a set of quicklinks using the current page's (or the specified page's) children as the destinations.
    This creates hierarchical lists up to two levels deep.
    The pages' titles are used as the link text and their summaries as tooltips.

## See also

- [Using macros](/en-US/docs/MDN/Writing_guidelines/Page_structures/Macros)
- [Macros](https://github.com/mdn/yari/tree/main/kumascript/macros) on Github
- [Commonly used macros](/en-US/docs/MDN/Writing_guidelines/Page_structures/Macros/Commonly_used_macros), including BCD macros ( `\{{Compat}}`, `\{{Compat(&lt;feature>)}}`, and `\{{Compat(&lt;feature>, &lt;depth>)}}`) and specification macros (`\{{Specifications}}` / `\{{Specifications(&lt;feature>)}}`)
- [Banners and notices guide](/en-US/docs/MDN/Writing_guidelines/Page_structures/Banners_and_notices)including the `\{{SeeCompatTable}}`, `\{{Deprecated_Header}}`, and `\{{SecureContext_Header}}` macros.
