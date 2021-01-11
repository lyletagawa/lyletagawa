# README.md

<https://lyletagawa.com/> was built using:
- [Hugo](https://gohugo.io/) open-source static site generator
- [Kiss](https://themes.gohugo.io/kiss/) "Stupidly simple" minimalist responsive Hugo theme
- [RealFaviconGenerator](https://realfavicongenerator.net/)

- Reading time:
  ```
  --- a/layouts/_default/single.html
  +++ b/layouts/_default/single.html
  @@ -13,7 +13,8 @@
       {{ partial "adsense" . }}
       {{ end }}
       <div class="content">
  +      <h6 class="subtitle is-6">Reading time: {{ .ReadingTime }} {{ cond (eq .ReadingTime 1) "minute" "minutes" }}</h6>
         {{ .Content }}
         {{ if .Site.Params.Info.related }}
         <div class="related">{{ partial "related" . }}</div>
         {{ end }}
  ```
