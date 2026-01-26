# SPARQLing Plant Metabolic Pathways Wiki

This repository contains the **SPARQLing Plant Metabolic Pathways Wiki** tutorial material, adapted from the original *SPARQLing Biology* workshop so that it can be reused in other workshops.

- 🌱 Online tutorial: <https://pathway-lod.github.io/SPARQLTutorials/>
- 🌐 PlantMetWiki SPARQL Explorer: <https://plantmetwiki.bioinformatics.nl/>

This tutorial was adapted from the course materials available at  
<https://DeniseSl22.github.io/SPARQLTutorials/>.

License for this tutorial and source code:  
[CC-BY-SA 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/legalcode)

## Credits

Authors of the original SPARQLing Biology material:

* Egon Willighagen  
* Marvin Martens  
* Denise Slenter  

We would like to acknowledge the material provided at  
<https://github.com/egonw/fvtworkshop> by Egon Willighagen, Ruud Steltenpool and Lars Willighagen, which has been used to construct this workshop (material is only available in Dutch).

Part of this material has been tested at the  
[BioSB conference breakout session](https://www.bigcat.unimaas.nl/sparqling-biology-breakout-session-at-biosb-2019/) taking place on the 3rd of April 2019 in Lunteren.  
The specific material for this workshop can be found at  
<https://bigcat-um.github.io/SPARQLTutorialBioSB2019/>.


---

## Serve this website locally for development (macOS, tested)

These instructions assume:

- macOS
- [Homebrew](https://brew.sh/) installed
- You are in this repository (e.g. `cd /path/to/SPARQLTutorials`)

The site uses Jekyll with the GitHub Pages theme **`jekyll-theme-tactile`** and is best run via **Bundler**, so the local environment matches GitHub Pages.

### 1. Install Ruby (via Homebrew)

```bash
brew install ruby
```

### 2. Ensure Homebrew Ruby is on your PATH

For Apple Silicon (M1/M2/M3):
```
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

For Intel Macs:
```
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

You can check Ruby with:
```
ruby -v
```

### 3. Install Bundler

```
gem install bundler
```

### 4. Install the site dependencies with Bundler

From the repo root 
(SPARQLTutorials):
    
```
cd /path/to/SPARQLTutorials
bundle install
```

This uses the Gemfile in the repository to install:
- jekyll
- jekyll-theme-tactile
- jekyll-seo-tag
- and any other required gems.


### 5. Serve the site locally (macOS)

### Why this setup
macOS ships with a locked “system Ruby” that often causes permission errors when installing gems.
To avoid this, use Homebrew Ruby and install gems locally in the repo.


#### 1 Install Ruby via Homebrew
```bash
brew install ruby
```

##### 2 Ensure Homebrew Ruby is used (IMPORTANT)
```bash 
echo 'export PATH="$(brew --prefix ruby)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

which ruby
ruby -v
```

If which ruby prints /usr/bin/ruby, you are still using system Ruby — fix your PATH before continuing.

#### 3  Install Bundler and dependencies (local install)
```bash 
gem install bundler

bundle config set --local path vendor/bundle
bundle install
```

#### 4 Run the site locally 

```bash 
bundle exec jekyll serve --port 4001
```

Jekyll will print something like:
```bash 
Server address: http://127.0.0.1:4001/
Server running... press ctrl-c to stop.
```

Open the URL in your browser (usually http://127.0.0.1:4000/￼).
You should see the tutorial rendered with the same tactile theme as on GitHub Pages.


## Feedback

If you have feedback on this tutorial or find an issue, please open a GitHub issue in this repository:

https://github.com/pathway-lod/SPARQLTutorials/issues￼