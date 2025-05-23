<template>

  <DocsPageTemplate>
    <DocsPageSection
      title="Overview"
      anchor="#overview"
    >
      <p>
        In general, we follow
        <DocsExternalLink
          href="https://material.io/archive/guidelines/style/writing.html"
          text="Material Design's writing guidelines"
        />. Our slightly adapted version is below.
      </p>
    </DocsPageSection>

    <DocsPageSection
      title="Language &amp; tone"
      anchor="#language"
    >
      <p>Most of these tips are the same as Material's guidelines:</p>
      <ul>
        <li>Address the user as 'you', and avoid using 'we'</li>
        <li>
          Use the
          <DocsExternalLink
            href="https://en.wikipedia.org/wiki/Singular_they"
            text="gender-neutral singular"
          />
          'they'
        </li>
        <li>Be concise</li>
        <li>Write in the present tense</li>
        <li>Write simply and directly</li>
        <li>Write for all levels of readers - do not use complex terminology</li>
        <li>Use consistent words in all parts of the app</li>
        <li>“1, 2, 3” not “one, two, three”</li>
        <li>Reveal detail as needed</li>
        <li>Be positive</li>
        <li>Communicate only essential details</li>
      </ul>
      <p>
        In Kolibri products, avoid creating new Proper Nouns in casual messages. For example, "You
        are a coach", not "You are a Coach".
        <DocsInternalLink
          href="/glossary"
          text="See also the Glossary page"
        />
        which outlines terms that have a specific meaning in the Kolibri product ecosystem.
      </p>
    </DocsPageSection>

    <DocsPageSection
      title="Punctuation"
      anchor="#punctuation"
    >
      <ul>
        <li>Use "Sentence case" capitalization for titles, not "Title Case" capitalization</li>
        <li>Skip periods and unnecessary punctuation</li>
        <li>Use contractions when they make a sentence easier to understand</li>
        <li>Avoid exclamation marks</li>
        <li>Always wrap user-generated text in 'single quotes'</li>
      </ul>
    </DocsPageSection>

    <DocsPageSection
      title="Internationalization"
      anchor="#i18n"
    >
      <p>
        All user-facing text and iconography needs to be internationalized. For text strings,
        numbers, dates, and times, this means "wrapping" the messages such that they can be mapped
        to messages in other languages using the
        <DocsExternalLink
          href="https://formatjs.io/guides/message-syntax/"
          text="ICU message syntax"
        />. See our
        <DocsExternalLink
          href="https://kolibri-dev.readthedocs.io/en/develop/i18n.html"
          text="developer docs"
        />
        for more information.
      </p>
      <p>
        Phrases that might not be globally understood should be avoided because they will be cause
        issues with translation.
      </p>
      <p>
        Note that currently our i18n framework does not support formatting such as bold or italics
        within a translated string.
      </p>
      <p>
        The built-in components in this guide support right-to-left languages out of the box. If you
        design or build custom components, these need to be fully tested using an RTL language. We
        use a
        <DocsExternalLink
          href="https://www.npmjs.com/package/webpack-rtl-plugin"
          text="webpack plugin"
        />
        to automatically flip most of the CSS, and the global <code>isRtl</code> attribute on all
        Vue components can be used to customize behavior if necessary.
      </p>
      <p>
        To properly support right-to-left languages and bi-directional text in custom components,
        follow the following guidelines:
      </p>
      <ul>
        <li>
          No displayed user data or content should have <code>right</code> or <code>left</code> set
          for the <code>text-align</code> property (<code>center</code> is OK)
        </li>
        <li>All displayed user data and content should have <code>dir="auto"</code></li>
        <li>
          <code>dir="auto"</code> should not generally be set on 'wrapper' nodes - it should be as
          close to the text as possible
        </li>
        <li>Translated "app text" does not generally need to have <code>dir="auto"</code></li>
      </ul>
    </DocsPageSection>
  </DocsPageTemplate>

</template>


<style lang="scss" scoped></style>
