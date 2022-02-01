# [Pourquoi Kubernetes ? Quels sont les bénéfices business et les difficultés à anticiper ?](https://www.linformaticien.com/partenaires/enix-io/59084-pourquoi-kubernetes-quels-sont-les-benefices-business-et-les-difficultes-a-anticiper.html)

 13 DÉCEMBRE 2021

-  
![](https://www.linformaticien.com/images/karatekong-inblogpost.png)

Depuis quelques années, de nouvelles technologies d'abstraction d'infrastructures permettent de **standardiser et faciliter le déploiement et l’opération d'applications ou de produits**. Parmi elles, les conteneurs et Kubernetes ont tiré leur épingle du jeu.

 Avant de parler de Kubernetes, rappelons qu'un conteneur est une entité logicielle qui englobe un processus ou une application, sa configuration et ses dépendances pour permettre son exécution sur n’importe quel environnement. **Kubernetes est la solution d'orchestration de ces conteneurs** applicatifs qui fait désormais référence. Pour mieux comprendre le fonctionnement et les aspects techniques de Kubernetes, vous pouvez lire ce billet d’introduction [_Kubernetes, c’est quoi?_](https://enix.io/fr/blog/kubernetes-c-est-quoi-definition-k8s/).

Dans cet article, nous nous concentrons sur l’**intérêt stratégique de Kubernetes** : quels sont les **bénéfices business et organisationnels** que vous pouvez en attendre ? Et quels sont les **points de vigilance** à anticiper ?

## **Les bénéfices de Kubernetes**

**La portabilité et l’interopérabilité**

Kubernetes est une solution **open source** pour déployer et gérer **vos applicatifs** **indépendamment de l'infrastructure IT** sous-jacente, avec ces bénéfices en termes d'implémentation :

-   Vous pouvez déployer votre plateforme Kubernetes (et donc vos applications, produits et services) sur tout type d’environnement : On-Premises, chez un hébergeur spécialisé, sur du Cloud privé, ou encore du Cloud Public. Vous **limitez ainsi le _vendor lock in_** _;_
-   Proposé par de nombreux acteurs, vous disposez d’une **grande flexibilité dans le choix de vos prestataires et de la solution Kubernetes** adaptée à votre stratégie cloud et hébergement : _Kubernetes Vanilla_ (sur mesure), les distributions Kubernetes (à déployer où vous le souhaitez), les services Kubernetes managés des hébergeurs spécialisés ou des fournisseurs de Cloud Public ;

-   L’indépendance vis-à-vis des fournisseurs d'infrastructures permet d’**adopter plus aisément des stratégies multi-cloud ou Cloud hybride** pour garantir une meilleure disponibilité de vos services et pour optimiser vos coûts auprès de ces prestataires.

**Accélérer le cycle de vie et le TTM de vos applications**

La migration à Kubernetes passe par la mise en conteneurs de vos applications et s’inscrit généralement dans une démarche plus globale d’adoption de l’approche DevOps. Kubernetes automatise et **accélère le déploiement de vos applications** sur les plateformes de test et de production. Couplé à l’utilisation d’une chaîne d’Intégration et de Déploiement continu (CI/CD), c’est l’ensemble du cycle de vie de vos applications qui sera automatisé et accéléré.

**Améliorer la disponibilité de votre service grâce au _self healing_**

Grâce à un système de sondes, en cas d'anomalie, **une plateforme Kubernetes est résiliente** : elle redémarre les conteneurs défaillants sans intervention humaine, améliorant ainsi la disponibilité de vos services.

**Permettre la scalabilité de votre plateforme en fonction de l’usage**

Kubernetes sélectionne automatiquement les serveurs qui gèrent les conteneurs applicatifs en fonction des ressources disponibles. Les mesures peuvent se baser sur la consommation des ressources matérielles (CPU, mémoire, etc.) et sur des métriques plus précises orientées métier. Ce _scheduling_ permet à la plateforme de “réagir” et de **supporter correctement la montée en charge** liée à des pics d’usage.

**Faciliter la maintenance et l’opération de votre plateforme**

Kubernetes permet la **gestion centralisée de votre plateforme**. On parle souvent des approches “_pet vs cattle”_ : au lieu d’intervenir sur chaque composant de votre infrastructure (_pet_), les équipes opérationnelles configurent et gèrent globalement l’ensemble de la plateforme (_cattle_).

La fonctionnalité de _scheduling_ peut aussi être **utile dans le cadre d’une maintenance de** l’infrastructure. L’opérateur peut par exemple déplacer aisément les groupes de conteneurs sur une partie de la plateforme Kubernetes pour libérer temporairement certains serveurs.

**Fluidifier les mises à jour des applications** 

Lors d'une mise à jour de votre application, Kubernetes va **déployer progressivement** la nouvelle version et s'assurer que le service est toujours rendu par les conteneurs nouvellement créés. En cas d'anomalie, le déploiement est stoppé pour conserver l'ancienne version et prévenir ainsi les risques en production. Kubernetes propose également nativement des mécanismes de _rollback_ permettant de **revenir rapidement** **à une version précédente** de votre application.

**Rendre les développeurs autonomes pour se concentrer sur leur métier**

Grâce à l’abstraction offerte par les conteneurs, les développeurs **n’ont plus à se soucier du déploiement et de l’infrastructure**, ils peuvent se concentrer sur le développement. Ils peuvent également travailler en local **sans nécessiter de disposer d’une réplique ou d’accéder** à la plateforme Kubernetes qui hébergera et exécutera leur code en production.

**Réduire les coûts opérationnels cloud et infrastructure**

Le passage aux conteneurs et à Kubernetes permet d’optimiser le coût de vos plateformes, a fortiori lorsque vous avez des applications avec de nombreux conteneurs et des architectures complexes :

-   Plus encore que la virtualisation de serveurs, les conteneurs permettent d’augmenter la densité d’applications par serveur, utilisant moins de ressources pour un même service ;
-   Grâce au _scheduling_ et au _scaling_ automatique des conteneurs par Kubernetes, l’**utilisation des ressources d’infrastructure est optimisée** : les conteneurs sont répartis intelligemment sur les composants, seules les ressources nécessaires à un instant t sont réservées ;
-   Kubernetes évite les silos et permet d’orchestrer de façon “unifiée” l’ensemble de vos applications. Vous **mutualisez une partie de vos coûts** de déploiement, d’opération, ou encore de monitoring.

## **Les points de vigilance avec Kubernetes**

**De nouvelles compétences opérationnelles**

Kubernetes et les conteneurs sont des technologies en rupture avec l’approche traditionnelle de développement des applications et d’opération des infrastructures. C’est une nouvelle façon d’opérer des plateformes qui nécessite des compétences spécifiques et un large spectre d’expertise. Pour réussir la transition, vous devrez donc **former vos équipes d'opérations** ou **recruter** de nouveaux profils. Il est bien sûr aussi possible de **déléguer ces tâches opérationnelles à un spécialiste de l’infogérance cloud native**, pour vous concentrer sur votre métier et le développement de vos applications et produits.

**Appréhender l'écosystème DevOps et Cloud Native**

Une plateforme Kubernetes interagit avec des composants externes spécialisés, en charge du stockage, des bases de données, des fonctions réseaux, du monitoring, etc. Déployer et opérer une plateforme Kubernetes nécessite de **maîtriser le fonctionnement de ces composants et leur intégration à la plateforme Kubernetes**. On peut citer par exemple les technologies Cloud Native : Helm, Prometheus, Harbor, Vault, Cilium. La chaîne d’intégration et de déploiement continu est également impactée par le passage aux conteneurs. Elle devra être mise à jour pour le déploiement des applications vers votre nouvel environnement Kubernetes.

**L’accompagnement des développeurs**

La conteneurisation de vos applications est un préalable à l’utilisation de Kubernetes. Vos développeurs devront être **accompagnés pour acquérir de nouvelles compétences et adopter de nouveaux outils**. Il leur faudra notamment produire les images de vos applications qui seront ensuite instanciées sous forme de conteneurs. 

**Les points de vigilance sur l’implémentation technique**

L’implémentation de Kubernetes est fortement liée à votre stratégie cloud et hébergement. L’architecture et la solution doivent aussi tenir compte de votre legacy, de vos applications et de votre métier. Il est donc indispensable d’avoir une réflexion globale en amont de cette transformation. Si vous souhaitez être accompagnés, vous pouvez vous appuyer sur des sociétés d’[expertise sur Kubernetes](https://enix.io/fr/kubernetes-devops-expert/). 

**La sécurité**

Comme pour les infrastructures traditionnelles, Kubernetes nécessite une réflexion poussée et une grande vigilance sur les aspects sécurité. Les principes sont similaires, avec des **spécificités pour des applications conteneurisées** orchestrées par Kubernetes : la recherche de vulnérabilités lors de la construction des images, la gestion des droits sur les conteneurs, la sécurisation réseau entre les conteneurs, etc.

## **Nos recommandations sur Kubernetes**

Kubernetes apporte des **bénéfices majeurs pour votre business** : sur la vélocité de livraison de vos applications, le confort de vos équipes, l’optimisation de votre budget infrastructure ou encore  la qualité et la disponibilité de vos services. Il faut cependant avoir à l’esprit que passer aux conteneurs et à Kubernetes est une **transformation profonde** qui implique des coûts de migration et dont l’implémentation peut s’avérer complexe.

Nous conseillons chez Enix.io d’opter pour une **approche pragmatique et itérative** dans la mise en place de votre plateforme Kubernetes :

-   Faites évoluer vos méthodes de développement, en adoptant les conteneurs et en considérant l’observabilité de vos applications pour faciliter leur opération ;
-   Sélectionnez la solution et l’implémentation de Kubernetes adaptés à votre métier en vous appuyant si nécessaire sur les bons prestataires pour l’hébergement/cloud, ou pour l’accompagnement à la transition et l’opération de votre plateforme ; 
-   Débutez avec une architecture Kubernetes simple que vous ferez évoluer par étapes (sur son périmètre fonctionnel, comme sur les applications gérées par la plateforme) ;
-   Déployez vos applications conteneurisées sur votre plateforme Kubernetes les unes après les autres, commencez par des applications simples. Il est indispensable de roder vos équipes et d’observer rapidement les bénéfices concrets de Kubernetes, pour éviter toute frustration dans la transition et en faire un succès pour votre société.