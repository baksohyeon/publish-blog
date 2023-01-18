---  
share: true  
title: Home  
  
---  
  
hide:  
  - navigation  
  - toc  
  
  
Mkdocs Obsidian is an association between an Obsidian plugin and a Material mkdocs template to get a personal wiki site based on your Obsidian Vault.  
  
<p align="center">  
	<a href="https://obsidian.md/"><img src="https://img.shields.io/badge/Auxiliary%20Tool-Obsidian-blueviolet"></img></a><br/>  
	<a href='https://ko-fi.com/X8X54ZYAV' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a><br/>  
	<a href="https://app.netlify.com/start/deploy?repository=https://github.com/ObsidianPublisher/publisher-template-gh-pages"><img src="https://www.netlify.com/img/deploy/button.svg"></a><br/>  
</p>  
  
- [Obsidian plugin](https://github.com/ObsidianPublisher/obsidian-github-publisher)  
- [Github Discussion](https://github.com/ObsidianPublisher/obsidian-github-publisher/discussions)  
  
### Support   
- [x] Wikilinks (`[[Links]]`)  
- [x] File transclusion/embed, both wikilinks and markdown links  
- [x] Obsidian callout and custom callout  
- [x] Folder notes and their citation  
- [x] Custom attributes  
- [x] Sharing state and custom folder hierarchy.  
- [x] Mobile and desktop  
- [x] File mini preview on Hover  
- [x] Graph view 🎉 (using [Obsidian Tools](https://github.com/mfarragher/obsidiantools))  
   
 ---  
## Github actions & Obsidian's plugin  
### TLDR  
1. Install the plugins through Obsidian Community or [BRAT](https://github.com/TfTHacker/obsidian42-brat)  
2. [Template](https://github.com/ObsidianPublisher/publisher-template-gh-pages) the blog and configure it   
3. Configure the plugin's options :   
	- Repo name  
	- Your github username  
	- The github token ([from here](https://github.com/settings/tokens/new?scopes=repo))  
	- The share key  
4. Add the sharing key in `true` in Obsidian's note frontmatter   
5. Customize (or not) the folder options   
6. Run the commands throught the file menu or commands palette.  
  
> [!INFO] [[Obsidian Github Publisher/Configuration example|See here for advanced configuration within obsidian]]  
  
---  
  
## Quick blog installation tutorial  
1. Click on[^1] :   
   - For Github Page : [use this template](https://github.com/ObsidianPublisher/publisher-template-gh-pages/generate)  
   - For Netlify : [use this template](https://github.com/ObsidianPublisher/publisher-template-netlify/generate)  
2. Use the name of your choice.  
3. Click on [code](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories) → SSH ; Copy the link  
4. Run (in terminal):  
```bash  
git clone [[PASTE THE LINK HERE]] publish_blog  
pip install -r requirements.txt  
```  
  
Also, don't forget to configure GitHub to allow push! [Check here for information about setting-up Git!](https://docs.github.com/en/get-started/quickstart/set-up-git)  
  
Alternatively, you can use the Netlify app to deploy your blog ! Just click here : [![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/ObsidianPublisher/publisher-template-netlify)   
Don't forget to update the `mkdocs.yml` !  
  
<p align="center">  
	<a href="https://www.netlify.com"> <img src="https://www.netlify.com/v3/img/components/netlify-color-bg.svg" alt="Deploys by Netlify" /> </a>  
</p>  
  
[^1]: You must be connected to copy the template ! You can test locally through clone > https : `git clone https://github.com/ObsidianPublisher/publisher-template-gh-pages` or [with downloading the ZIP](https://github.com/ObsidianPublisher/publisher-template-gh-pages/archive/refs/heads/main.zip)  
