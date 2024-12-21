---
title: "All Tags"
---

{{ range $key, $value := .Site.Taxonomies.tags }}
<a href="/tags/{{ $key | urlize }}">{{ $key }} ({{ len $value }})</a><br>
{{ end }}

