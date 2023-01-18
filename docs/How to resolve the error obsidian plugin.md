---  
share: true  
---  
`What should I do for another vault`   
1. modify requirements.txt:   
  
```json  
mkdocs==1.4.2  
mkdocs-material==9.0.5  
mkdocs-mermaid2-plugin==0.6.0  
mkdocs-ezlinked-plugin==0.3.3  
mkdocs-awesome-pages-plugin==2.8.0  
mdx_breakless_lists==1.0.1  
mkdocs-embed-file-plugins==2.0.5  
mkdocs_custom_fences==0.1.2  
mkdocs-git-revision-date-localized-plugin==1.1.0  
mkdocs-encryptcontent-plugin==2.4.4  
mkdocs-callouts==1.8.1  
mkdocs-custom-tags-attributes==0.3.0  
mkdocs-static-i18n==0.53  
mkdocs-meta-descriptions-plugin==2.2.0  
obsidiantools==0.8.1  
pyvis==0.3.1  
mkdocs-glightbox==0.3.1  
mkdocs-minify-plugin==0.6.2  
```  
  
2. modify main.js  https://discord.com/channels/1012652786887954443/1012653060230754394/1064860285476798485   
       Just download the file and put in with remplacement in the plugin folder  
![main.js](./main.js.md)  
3. add file `tags`   
![Pasted image 20230117211746.png](./Pasted%20image%2020230117211746.png)  
  
4. edit header   
You can change the header for each category (as far I know) but you can change the background by changing this line in the customization.css :  
```css  
.md-header,  
.md-nav--primary .md-nav__title[for="__drawer"] {  
    background: var(--md-primary-fg-color) url(https://images.unsplash.com/photo-1520157646479-22f25713523d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1074&q=80) left center/cover no-repeat;  
}  
```  
  
  
  
docs/index → home  
category: something → 카테고리화 됨  
  
[TAGS] 있으면 문서 인식 못함