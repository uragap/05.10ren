---
"$title": Spécification AMP HTML
order: '8'
formats:
- sites Internet
teaser:
  text: " AMP HTML est un sous-ensemble de HTML pour la création de pages de contenu telles que les actualités articles d'une manière qui garantit certaines performances de base caractéristiques."
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-html-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

AMP HTML est un sous-ensemble de HTML permettant de créer des pages de contenu telles que des articles de presse d'une manière qui garantit certaines caractéristiques de performances de base.

Étant un sous-ensemble de HTML, il impose des restrictions sur l'ensemble complet des balises et des fonctionnalités disponibles via HTML, mais il ne nécessite pas le développement de nouveaux moteurs de rendu: les agents utilisateurs existants peuvent rendre AMP HTML comme tous les autres HTML.

[tip type="read-on"] Si vous êtes principalement intéressé par ce qui est autorisé dans AMP et par ce qui ne l'est pas, regardez notre [vidéo d'introduction sur les limitations d'AMP](https://www.youtube.com/watch?v=Gv8A4CktajQ) . [/tip]

De plus, les documents AMP HTML peuvent être téléchargés sur un serveur Web et servis comme n'importe quel autre document HTML; aucune configuration spéciale pour le serveur n'est nécessaire. Cependant, ils sont également conçus pour être éventuellement servis via des systèmes de service AMP spécialisés qui procèdent à des proxy de documents AMP. Ces documents leur servent de leur propre origine et sont autorisés à appliquer des transformations au document qui offrent des avantages de performance supplémentaires. Une liste incomplète des optimisations qu'un tel système de diffusion pourrait faire est la suivante:

- Remplacez les références d'image par des images dimensionnées selon la fenêtre de la visionneuse
- Images en ligne visibles au-dessus du pli.
- Variables CSS en ligne.
- Préchargez les composants étendus.
- Réduisez le HTML et le CSS.

AMP HTML utilise un ensemble d'éléments personnalisés contribués mais gérés et hébergés de manière centralisée pour implémenter des fonctionnalités avancées telles que des galeries d'images pouvant être trouvées dans un document AMP HTML. Bien qu'il permette de styliser le document à l'aide de CSS personnalisé, il ne permet pas à l'auteur d'écrire du JavaScript au-delà de ce qui est fourni par les éléments personnalisés pour atteindre ses objectifs de performance.

En utilisant le format AMP, les producteurs de contenu rendent le contenu des fichiers AMP disponible pour l'exploration (sous réserve des restrictions du fichier robots.txt), la mise en cache et l'affichage par des tiers.

## Performance<a name="performance"></a>

Des performances prévisibles sont un objectif de conception clé pour AMP HTML. Nous visons principalement à réduire le temps jusqu'à ce que le contenu d'une page puisse être consommé / utilisé par l'utilisateur. Concrètement, cela signifie que:

- Les requêtes HTTP nécessaires au rendu et à la mise en page complète du document doivent être réduites au minimum.
- Les ressources telles que les images ou les publicités ne doivent être téléchargées que si elles sont susceptibles d'être vues par l'utilisateur.
- Les navigateurs doivent être en mesure de calculer l'espace nécessaire à chaque ressource sur la page sans récupérer cette ressource.

## Le format AMP HTML<a name="the-amp-html-format"></a>

### Exemple de document<a name="sample-document"></a>

[sourcecode:html] <!DOCTYPE html> <html ⚡>   <head>     <meta charset="utf-8" />     <title>Sample document</title>     <link rel="canonical" href="./regular-html-version.html" />     <meta       name="viewport"       content="width=device-width,minimum-scale=1,initial-scale=1"     />     <style amp-custom>       h1 {         color: red;       }     </style>     <script type="application/ld+json">       {         "@context": "http://schema.org",         "@type": "NewsArticle",         "headline": "Article headline",         "image": ["thumbnail1.jpg"],         "datePublished": "2015-02-05T08:00:00+08:00"       }     </script>     <script       async       custom-element="amp-carousel"       src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"     ></script>     <script       async       custom-element="amp-ad"       src="https://cdn.ampproject.org/v0/amp-ad-0.1.js"     ></script>     <style amp-boilerplate>       body {         -webkit-animation: -amp-start 8s steps(1, end) 0s 1 normal both;         -moz-animation: -amp-start 8s steps(1, end) 0s 1 normal both;         -ms-animation: -amp-start 8s steps(1, end) 0s 1 normal both;         animation: -amp-start 8s steps(1, end) 0s 1 normal both;       }       @-webkit-keyframes -amp-start {         from {           visibility: hidden;         }         to {           visibility: visible;         }       }       @-moz-keyframes -amp-start {         from {           visibility: hidden;         }         to {           visibility: visible;         }       }       @-ms-keyframes -amp-start {         from {           visibility: hidden;         }         to {           visibility: visible;         }       }       @-o-keyframes -amp-start {         from {           visibility: hidden;         }         to {           visibility: visible;         }       }       @keyframes -amp-start {         from {           visibility: hidden;         }         to {           visibility: visible;         }       }     </style>     <noscript       ><style amp-boilerplate>         body {           -webkit-animation: none;           -moz-animation: none;           -ms-animation: none;           animation: none;         }       </style></noscript     >     <script async src="https://cdn.ampproject.org/v0.js"></script>   </head>   <body>     <h1>Sample document</h1>     <p>       Some text       <amp-img src="sample.jpg" width="300" height="300"></amp-img>     </p>     <amp-ad       width="300"       height="250"       type="a9"       data-aax_size="300x250"       data-aax_pubname="test123"       data-aax_src="302"     >     </amp-ad>   </body> </html> [/sourcecode]

### Balisage requis<a name="required-markup"></a>

Les documents AMP HTML DOIVENT

- <a name="dctp"></a>commencez par le doctype `<!doctype html>` . [🔗] (# dctp)

- <a name="ampd"></a>contiennent une `<html ⚡>` ( `<html amp>` est également accepté). [🔗] (# ampd)

- <a name="crps"></a>contiennent les `<head>` et `<body>` (elles sont facultatives en HTML). [🔗] (# crps)

- <a name="canon"></a>contiennent une `<link rel="canonical" href="$SOME_URL">` dans leur tête qui pointe vers la version HTML standard du document AMP HTML ou vers lui-même si une telle version HTML n'existe pas. [🔗] (# canon)

- <a name="chrs"></a>contiennent une `<meta charset="utf-8">` comme premier enfant de leur balise head. [🔗] (# chrs)

- <a name="vprt"></a>contiennent une `<meta name="viewport" content="width=device-width">` l'intérieur de leur balise head. Il est également recommandé d'inclure `minimum-scale=1` et `initial-scale=1` . [🔗] (# vprt)

- <a name="scrpt"></a>contiennent une `<script async src="https://cdn.ampproject.org/v0.js"></script>` dans leur balise head. [🔗] (# scrpt)

- <a name="boilerplate"></a>contiennent le [code standard AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) ( `head > style[amp-boilerplate]` et `noscript > style[amp-boilerplate]` ) dans leur balise head. [🔗] (# passe-partout)

### Métadonnées<a name="metadata"></a>

Il est recommandé que les documents AMP HTML soient annotés avec des métadonnées standardisées: [protocole Open Graph](http://ogp.me/) , [cartes Twitter](https://dev.twitter.com/cards/overview) , etc.

Nous recommandons également que les documents AMP HTML soient balisés avec [schema.org/CreativeWork](https://schema.org/CreativeWork) ou l'un de ses types plus spécifiques tels que [schema.org/NewsArticle](https://schema.org/NewsArticle) ou [schema.org/BlogPosting](https://schema.org/BlogPosting) .

### Balises HTML<a name="html-tags"></a>

Les balises HTML peuvent être utilisées telles quelles dans AMP HTML. Certaines balises ont des balises personnalisées équivalentes (telles que `<img>` et `<amp-img>` ) et d'autres balises sont carrément interdites:

<table>
  <tr>
    <th width="30%">Étiqueter</th>
    <th>Statut en AMP HTML</th>
  </tr>
  <tr>
    <td width="30%">scénario</td>
    <td>Interdit sauf si le type est <code>application/ld+json</code> , <code>application/json</code> ou <code>text/plain</code> . (D'autres valeurs non exécutables peuvent être ajoutées si nécessaire.) L'exception est la balise de script obligatoire pour charger le moteur d'exécution AMP et les balises de script pour charger les composants étendus.</td>
  </tr>
  <tr>
    <td width="30%">noscript</td>
    <td>Autorisé. Peut être utilisé n'importe où dans le document. S'il est spécifié, le contenu de l' <code>&lt;noscript&gt;</code> s'affiche si JavaScript est désactivé par l'utilisateur.</td>
  </tr>
  <tr>
    <td width="30%">base</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">img</td>
    <td>Remplacé par <code>amp-img</code> .<br> Remarque: <code>&lt;img&gt;</code> est un <a href="https://www.w3.org/TR/html5/syntax.html#%20void-elements">élément vide selon HTML5</a> , il n'a donc pas de balise de fin. Cependant, <code>&lt;amp-img&gt;</code> a une balise de fin <code>&lt;/amp-img&gt;</code> .</td>
  </tr>
    <tr>
    <td width="30%">image</td>
    <td>Interdit. Servez différents formats d'image en utilisant l' <a href="https://amp.dev/documentation/guides-and-tutorials/develop/style_and_layout/placeholders?format=websites">attribut de secours</a> ou fournissez plusieurs <a href="https://amp.dev/documentation/components/amp-img#%20attributes"><code>srcset</code> sur <code>&lt;amp-img&gt;</code></a> .</td>
  </tr>
  <tr>
    <td width="30%">vidéo</td>
    <td>Remplacé par <code>amp-video</code> .</td>
  </tr>
  <tr>
    <td width="30%">l'audio</td>
    <td>Remplacé par <code>amp-audio</code> .</td>
  </tr>
  <tr>
    <td width="30%">iframe</td>
    <td>Remplacé par <code>amp-iframe</code> .</td>
  </tr>
    <tr>
    <td width="30%">Cadre</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">jeu de cadres</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">objet</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">param</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">applet</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">intégrer</td>
    <td>Interdit.</td>
  </tr>
  <tr>
    <td width="30%">forme</td>
    <td>Autorisé. Nécessite l'inclusion d'une extension <a href="https://amp.dev/documentation/components/amp-form">amp-form.</a>
</td>
  </tr>
  <tr>
    <td width="30%">éléments d'entrée</td>
    <td>Surtout autorisé à l' <a href="https://amp.dev/documentation/components/amp-form#%20inputs-and-fields">exception de certains types d'entrée</a> , à savoir, <code>&lt;input type=button&gt;</code> , <code>&lt;button type=image&gt;</code> sont pas valides. Les balises associées sont également autorisées: <code>&lt;fieldset&gt;</code> , <code>&lt;label&gt;</code>
</td>
  </tr>
  <tr>
    <td width="30%">bouton</td>
    <td>Autorisé.</td>
  </tr>
  <tr>
    <td width="30%"><code>&lt;a name="cust"&gt;&lt;/a&gt;style</code></td>
    <td>
<a href="#%20boilerplate">Étiquette de style requise pour amp-passe-partout</a> . Une balise de style supplémentaire est autorisée dans la balise head à des fins de style personnalisé. Cette balise de style doit avoir l'attribut <code>amp-custom</code> . <a href="#cust">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">lien</td>
    <td>
<code>rel</code> valeurs rel enregistrées sur <a href="http://microformats.org/wiki/existing-rel-values">microformats.org</a> sont autorisées. S'il <code>rel</code> dans notre liste blanche, <a href="https://github.com/ampproject/amphtml/issues/new">veuillez soumettre un problème</a> . <code>stylesheet</code> et d'autres valeurs telles que la <code>preconnect</code> , la <code>prerender</code> et la <code>prefetch</code> qui ont des effets secondaires dans le navigateur ne sont pas autorisées. Il existe un cas particulier pour récupérer les feuilles de style auprès des fournisseurs de polices de la liste blanche.</td>
  </tr>
  <tr>
    <td width="30%">méta</td>
    <td>L' <code>http-equiv</code> peut être utilisé pour des valeurs autorisées spécifiques; voir la <a href="https://github.com/ampproject/amphtml/blob/master/validator/validator-main.protoascii">spécification du validateur AMP</a> pour plus de détails.</td>
  </tr>
  <tr>
    <td width="30%"><code>&lt;a name="ancr"&gt;&lt;/a&gt;a</code></td>
    <td>La <code>href</code> attribut href ne doit pas commencer par <code>javascript:</code> S'il est défini, la valeur de l'attribut <code>target</code> <code>_blank</code> . Autrement autorisé. <a href="#%20ancr">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">svg</td>
    <td>La plupart des éléments SVG sont autorisés.</td>
  </tr>
</table>

Les implémentations du validateur doivent utiliser une liste blanche basée sur la spécification HTML5 avec les balises ci-dessus supprimées. Voir l' [addendum sur les balises AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-tag-addendum.md) .

### commentaires<a name="comments"></a>

Les commentaires HTML conditionnels ne sont pas autorisés.

### Attributs HTML<a name="html-attributes"></a>

Les noms d'attributs commençant par `on` (comme `onclick` ou `onmouseover` ) ne sont pas autorisés dans AMP HTML. L'attribut avec le nom littéral `on` (pas de suffixe) est autorisé.

Les attributs XML, tels que xmlns, xml: lang, xml: base et xml: space ne sont pas autorisés dans AMP HTML.

Les attributs AMP internes préfixés par `i-amp-` ne sont pas autorisés dans AMP HTML.

### Des classes<a name="classes"></a>

Les noms de classe AMP internes précédés de `-amp-` et `i-amp-` ne sont pas autorisés dans AMP HTML.

Consultez la [documentation AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-css-classes.md) `amp-` signification des noms de classe précédés de amp-. L'utilisation de ces classes est autorisée et destinée à permettre la personnalisation de certaines fonctionnalités de l'environnement d'exécution et des extensions AMP.

Tous les autres noms de classe créés sont autorisés dans le balisage HTML AMP.

### ID<a name="ids"></a>

Certains noms d'ID ne sont pas autorisés dans AMP HTML, tels que les ID précédés de `-amp-` et `i-amp-` qui peuvent entrer en conflit avec les ID AMP internes.

`amp-` extensions spécifiques avant d'utiliser les ID amp et `AMP` afin d'éviter tout conflit avec les fonctionnalités fournies par ces extensions, telles que `amp-access` .

Consultez la liste complète des noms d'ID non autorisés en recherchant `mandatory-id-attr` [ici](https://github.com/ampproject/amphtml/blob/master/spec/../validator/validator-main.protoascii)

### Liens<a name="links"></a>

Le `javascript:` n'est pas autorisé.

### Feuilles de style<a name="stylesheets"></a>

Les principales balises sémantiques et les éléments personnalisés AMP sont fournis avec des styles par défaut pour faciliter la création d'un document réactif. Une option pour désactiver les styles par défaut peut être ajoutée à l'avenir.

#### @-des règles<a name="-rules"></a>

Les @ -rules suivantes sont autorisées dans les feuilles de style:

`@font-face` , `@keyframes` , `@media` , `@page` , `@supports` .

`@import` ne sera pas autorisé. D'autres peuvent être ajoutés à l'avenir.

#### Feuilles de style de l'auteur<a name="author-stylesheets"></a>

Les auteurs peuvent ajouter des styles personnalisés à un document en utilisant une seule `<style amp-custom>` dans l'en-tête du document ou des styles en ligne.

`@keyframes` règles @keyframes sont autorisées dans le `<style amp-custom>` . Cependant, s'ils sont trop nombreux, il est recommandé de les placer dans la `<style amp-keyframes>` , qui doit se trouver à la fin du document AMP. Pour plus de détails, reportez-vous à la section [Feuille de style des images clés] (# feuille de style des images clés) de ce document.

#### Sélecteurs<a name="selectors"></a>

Les restrictions suivantes s'appliquent aux sélecteurs dans les feuilles de style d'auteur:

##### Noms de classe et d'étiquette<a name="class-and-tag-names"></a>

Les noms de classe, les ID, les noms de balises et les attributs, dans les feuilles de style de l'auteur, peuvent ne pas commencer par la chaîne `-amp-` et `i-amp-` . Celles-ci sont réservées à un usage interne par le runtime AMP. Il s'ensuit que la feuille de style de l'utilisateur peut ne pas faire référence aux sélecteurs CSS pour les `-amp-` , les `i-amp-` ID et les balises et attributs `i-amp-` Ces classes, ID et noms de balises / attributs ne sont pas destinés à être personnalisés par les auteurs. Les auteurs, cependant, peuvent remplacer les styles des `amp-` et des balises pour toutes les propriétés CSS qui ne sont pas explicitement interdites par les spécifications de ces composants.

Pour empêcher l'utilisation des sélecteurs d'attributs pour contourner les limitations de nom de classe, il n'est généralement pas permis aux sélecteurs CSS de contenir des jetons et des chaînes commençant par `-amp-` et `i-amp-` .

#### Important<a name="important"></a>

L'utilisation du `!important` n'est pas autorisée. C'est une condition nécessaire pour permettre à AMP d'appliquer ses invariants de dimensionnement d'élément.

#### Propriétés<a name="properties"></a>

AMP autorise uniquement les transitions et les animations de propriétés qui peuvent être accélérées par GPU dans les navigateurs courants. Nous avons actuellement une liste blanche: `opacity` , `transform` (également `-vendorPrefix-transform` ).

Dans les exemples suivants, `<property>` doit figurer dans la liste blanche ci-dessus.

- `transition <property>` (également -vendorPrefix-transition)
- `@keyframes name { from: {<property>: value} to {<property: value>} }` (également `@-vendorPrefix-keyframes` )

#### Taille maximum<a name="maximum-size"></a>

Il s'agit d'une erreur de validation si la feuille de style de l'auteur ou les styles en ligne ensemble sont supérieurs à 75 000 octets.

### Feuille de style des images clés<a name="keyframes-stylesheet"></a>

En plus de la `<style amp-custom>` , les auteurs peuvent également ajouter la `<style amp-keyframes>` , qui est autorisée spécifiquement pour les animations d'images clés.

Les restrictions suivantes s'appliquent à la `<style amp-keyframes>` :

1. Ne peut être placé que comme dernier enfant de l'élément `<body>`
2. Ne peut contenir que des `@keyframes` , `@media` , `@supports` et leur combinaison.
3. Ne peut pas dépasser 500 000 octets.

La raison pour laquelle la `<style amp-keyframes>` existe est parce que les règles des images clés sont souvent volumineuses, même pour des animations moyennement compliquées, ce qui conduit à une analyse CSS lente et à une première peinture pleine de contenu. Mais ces règles dépassent souvent la limite de taille imposée à `<style amp-custom>` . Mettre de telles déclarations d'images clés au bas du document dans les `<style amp-keyframes>` leur permet de dépasser les limites de taille. Et comme les images clés ne bloquent pas le rendu, cela évite également de bloquer la première peinture de contenu pour les analyser.

Exemple:

[sourcecode:html] <style amp-keyframes> @keyframes anim1 {}  @media (min-width: 600px) {   @keyframes anim1 {} } </style> </body> [/sourcecode]

### Polices personnalisées<a name="custom-fonts"></a>

Les auteurs peuvent inclure des feuilles de style pour les polices personnalisées. Les 2 méthodes prises en charge sont les balises de lien pointant vers les fournisseurs de polices en liste blanche et l'inclusion `@font-face`

Exemple:

[sourcecode:html] <link   rel="stylesheet"   href="https://fonts.googleapis.com/css?family=Tangerine" /> [/sourcecode]

Les fournisseurs de polices peuvent figurer sur la liste blanche s'ils prennent en charge les intégrations CSS uniquement et servent via HTTPS. Les origines suivantes sont actuellement autorisées pour la diffusion de polices via des balises de lien:

- Fonts.com: `https://fast.fonts.net`
- Google Fonts: `https://fonts.googleapis.com`
- Font Awesome: `https://maxcdn.bootstrapcdn.com, https://use.fontawesome.com`
- [Typekit](https://helpx.adobe.com/typekit/using/google-amp.html) : `https://use.typekit.net/kitId.css` (remplacez `kitId` conséquence)

REMARQUE POUR LES IMPLÉMENTAIRES: l'ajout à cette liste nécessite une modification de la règle AMP Cache CSP.

Les auteurs sont libres d'inclure toutes les polices personnalisées via une `@font-face` via leur CSS personnalisé. Les polices incluses via `@font-face` doivent être récupérées via le schéma HTTP ou HTTPS.

## Runtime AMP<a name="amp-runtime"></a>

Le runtime AMP est un morceau de JavaScript qui s'exécute à l'intérieur de chaque document AMP. Il fournit des implémentations pour les éléments personnalisés AMP, gère le chargement et la hiérarchisation des ressources et inclut éventuellement un validateur d'exécution pour AMP HTML à utiliser pendant le développement.

Le runtime AMP est chargé via la `<script src="https://cdn.ampproject.org/v0.js"></script>` dans le document AMP `<head>` .

Le moteur d'exécution AMP peut être placé dans un mode de développement pour n'importe quelle page. Le mode de développement déclenchera la validation AMP sur la page intégrée, qui émettra l'état de validation et toutes les erreurs à la console du développeur JavaScript. Le mode de développement peut être déclenché en ajoutant `# development=1` à l'URL de la page.

## Ressources<a name="resources"></a>

Des ressources telles que des images, des vidéos, des fichiers audio ou des publicités doivent être incluses dans un fichier AMP HTML via des éléments personnalisés tels que `<amp-img>` . Nous les appelons «ressources gérées» car le moment et le moment où elles seront chargées et affichées à l'utilisateur est décidé par le moteur d'exécution AMP.

Il n'y a pas de garanties particulières quant au comportement de chargement du runtime AMP, mais il doit généralement s'efforcer de charger les ressources assez rapidement, afin qu'elles soient chargées au moment où l'utilisateur voudrait les voir si possible. Le moteur d'exécution doit hiérarchiser les ressources actuellement dans la fenêtre et tenter de prédire les modifications apportées à la fenêtre et précharger les ressources en conséquence.

Le moteur d'exécution AMP peut à tout moment décider de décharger des ressources qui ne sont pas actuellement dans la fenêtre d'affichage ou de réutiliser les conteneurs de ressources tels que les iframes pour réduire la consommation globale de RAM.

## Composants AMP<a name="amp-components"></a>

AMP HTML utilise des éléments personnalisés appelés «composants AMP» pour remplacer les balises de chargement de ressources intégrées telles que `<img>` et `<video>` et pour implémenter des fonctionnalités avec des interactions complexes telles que des visionneuses d'images ou des carrousels.

Reportez-vous aux [spécifications des composants AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-components.md) pour plus de détails sur les composants pris en charge.

Il existe 2 types de composants AMP pris en charge:

1. Intégré
2. Élargi

Les composants intégrés sont toujours disponibles dans un document AMP et ont un élément personnalisé dédié tel que `<amp-img>` . Les composants étendus doivent être explicitement inclus dans le document.

### Attributs communs<a name="common-attributes"></a>

#### `layout` , `width` , `height` , `media` , `placeholder` , `fallback`<a name="layout-width-height-media-placeholder-fallback"></a>

Ces attributs définissent la disposition d'un élément. L'objectif principal ici est de s'assurer que l'élément peut être affiché et que son espace peut être correctement réservé avant le téléchargement de toute ressource JavaScript ou distante.

Voir le [système de mise en page AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-layout.md) pour plus de détails sur le système de mise en page.

#### `on` <a name="on"></a>

L' `on` attribut est utilisé pour installer des gestionnaires d'événements sur les éléments. Les événements pris en charge dépendent de l'élément.

La valeur de la syntaxe est un simple langage spécifique au domaine du formulaire:

[sourcecode:javascript] eventName:targetId[.methodName[(arg1=value, arg2=value)]] [/sourcecode]

Exemple: `on="tap:fooId.showLightbox"`

Si `methodName` est omis, la méthode par défaut est exécutée si elle est définie pour l'élément. Exemple: `on="tap:fooId"`

Certaines actions, si elles sont documentées, peuvent accepter des arguments. Les arguments sont définis entre parenthèses dans la notation `key=value` Les valeurs acceptées sont:

- chaînes simples sans guillemets: `simple-value` ;
- chaînes entre guillemets: `"string value"` ou `'string value'` ;
- valeurs booléennes: `true` ou `false` ;
- nombres: `11` ou `1.1` .

Vous pouvez écouter plusieurs événements sur un élément en séparant les deux événements par un point-virgule `;` .

Exemple: `on="submit-success:lightbox1;submit-error:lightbox2"`

En savoir plus sur les [actions et événements AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-actions-and-events.md) .

### Composants étendus<a name="extended-components"></a>

Les composants étendus sont des composants qui ne sont pas nécessairement fournis avec l'environnement d'exécution AMP. Au lieu de cela, ils doivent être explicitement inclus dans le document.

Les composants étendus sont chargés en incluant une `<script>` dans l'en-tête du document comme ceci:

[sourcecode:html] <script   async   custom-element="amp-carousel"   src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js" ></script> [/sourcecode]

La `<script>` doit avoir un `async` et doit avoir un `custom-element` référençant le nom de l'élément.

Les implémentations d'exécution peuvent utiliser le nom pour rendre les espaces réservés pour ces éléments.

L'URL du script doit commencer par `https://cdn.ampproject.org` et doit suivre un modèle très strict de / `/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js` .

##### URL<a name="url"></a>

L'URL des composants étendus est de la forme:

[sourcecode:http] https://cdn.ampproject.org/$RUNTIME_VERSION/$ELEMENT_NAME-$ELEMENT_VERSION.js [/sourcecode]

##### Gestion des versions<a name="versioning"></a>

Consultez la [politique de contrôle de version AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-versioning-policy.md) .

### Modèles<a name="templates"></a>

Les modèles rendent le contenu HTML basé sur le modèle spécifique à la langue et les données JSON fournies.

Consultez les [spécifications du modèle AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md) pour plus de détails sur les modèles pris en charge.

Les modèles ne sont pas livrés avec le moteur d'exécution AMP et doivent être téléchargés comme avec les éléments étendus. Les composants étendus sont chargés en incluant une `<script>` dans l'en-tête du document comme ceci:

[sourcecode:html] <script   async   custom-template="amp-mustache"   src="https://cdn.ampproject.org/v0/amp-mustache-0.2.js" ></script> [/sourcecode]

La `<script>` doit avoir un `async` et doit avoir un `custom-template` référençant le type du modèle. L'URL du script doit commencer par `https://cdn.ampproject.org` et doit suivre un modèle très strict de / `/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js` .

Les modèles sont déclarés dans le document comme suit:

[sourcecode:html] <template type="amp-mustache" id="template1">   Hello {% raw %}{{you}}{% endraw %}! </template> [/sourcecode]

L' `type` est obligatoire et doit référencer un script de `custom-template`

L' `id` est facultatif. Les éléments AMP individuels découvrent leurs propres modèles. Les scénarios typiques impliqueraient un élément AMP à la recherche d'un `<template>` parmi ses enfants ou référencé par ID.

La syntaxe de l'élément de modèle dépend du langage de modèle spécifique. Cependant, la langue du modèle peut être restreinte dans AMP. Par exemple, conformément à l'élément "template", toutes les productions doivent être sur un DOM valide bien formé. Toutes les sorties de modèle sont également soumises à un nettoyage pour garantir une sortie valide pour AMP.

Pour en savoir plus sur la syntaxe et les restrictions d'un modèle, visitez la [documentation du modèle] (https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md# templates).

##### URL<a name="url-1"></a>

L'URL des composants étendus est de la forme:

[sourcecode:http] https://cdn.ampproject.org/$RUNTIME_VERSION/$TEMPLATE_TYPE-$TEMPLATE_VERSION.js [/sourcecode]

##### Gestion des versions<a name="versioning-1"></a>

Voir la gestion des versions des éléments personnalisés pour plus de détails.

## Sécurité<a name="security"></a>

Les documents AMP HTML ne doivent pas déclencher d'erreurs lorsqu'ils sont diffusés avec une politique de sécurité du contenu qui n'inclut pas les mots `unsafe-inline` et `unsafe-eval` .

Le format AMP HTML est conçu pour que ce soit toujours le cas.

Tous les éléments du modèle AMP doivent passer par un examen de sécurité AMP avant de pouvoir être soumis dans le référentiel AMP.

## SVG<a name="svg"></a>

Actuellement, les éléments SVG suivants sont autorisés:

- notions de base: "g", "glyph", "glyphRef", "image", "marker", "metadata", "path", "solidcolor", "svg", "switch", "view"
- formes: "cercle", "ellipse", "ligne", "polygone", "polyligne", "rect"
- text: "text", "textPath", "tref", "tspan"
- rendu: "clipPath", "filter", "hkern", "linearGradient", "mask", "pattern", "radialGradient", "vkern"
- special: "defs" (tous les enfants ci-dessus sont autorisés ici), "symbol", "use"
- filtre: "feColorMatrix", "feComposite", "feGaussianBlur", "feMerge", "feMergeNode", "feOffset", "ForeignObject"
- ARIA: "desc", "title"

En plus de ces attributs:

- "xlink: href": seuls les URI commençant par "#" sont autorisés

- "style"

## Découverte de documents AMP<a name="amp-document-discovery"></a>

Le mécanisme décrit ci-dessous fournit un moyen standardisé pour le logiciel de découvrir si une version AMP existe pour un document canonique.

S'il existe un document AMP qui est une représentation alternative d'un document canonique, alors le document canonique doit pointer vers le document AMP via une `link` avec la [relation "amphtml"] (http://microformats.org/wiki/existing- rel-values # HTML5_link_type_extensions).

Exemple:

[sourcecode:html] <link rel="amphtml" href="https://www.example.com/url/to/amp/document.html" /> [/sourcecode]

On s'attend à ce que le document AMP lui-même renvoie à son document canonique via une `link` avec la relation «canonique».

Exemple:

[sourcecode:html] <link   rel="canonical"   href="https://www.example.com/url/to/canonical/document.html" /> [/sourcecode]

(Si une seule ressource est simultanément l'AMP *et* le document canonique, la relation canonique doit pointer vers elle-même - aucune relation «amphtml» n'est requise.)

Notez que pour une compatibilité plus large avec les systèmes consommateurs d'AMP, il devrait être possible de lire la relation "amphtml" sans exécuter JavaScript. (Autrement dit, la balise doit être présente dans le HTML brut et non injectée via JavaScript.)
