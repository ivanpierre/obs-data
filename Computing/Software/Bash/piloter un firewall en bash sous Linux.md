# piloter un firewall en bash sous Linux

  
By [Christophe Casalegno](https://www.christophe-casalegno.com/author/admin/ "View all posts by Christophe Casalegno") on janvier 9, 2022 in [admin sys / network / linux](https://www.christophe-casalegno.com/category/admin-sys-network-linux/), [Infosec](https://www.christophe-casalegno.com/category/infosec/)

Hello, j’avais dit lors de cette vidéo que je mettrai le script en ligne après relecture. Bon je n’ai pas eu le temps de le relire et de le faire au propre, mais il est fonctionnel. Si vous souhaitez en savoir plus à son sujet, je vous invite à regarder cette vidéo :

 [Livecoding : un firewall en bash sous Linux - YouTube](https://www.youtube.com/watch?v=rsEHP5-Y3Zo)
 
Son, utilisation est très simple :

1) Pour blocker une adresse IP

```
 ./brainfw.sh block ip 192.168.1.15 
```

  
   
2) Pour blocker une plage d’adresse

```
 ./brainfw block ip 192.168.0.0/24 
```

  
   
3) Pour blocker un pays

```
 ./brainfw block country countrycode (exemple : fr pour France) 
```

  
   
Pour débloquer, c’est la même chose mais en utilisant « unblock » en paramètre au lieu de « block ». Vous pouvez le télécharger via le lien suivant : [brainfw.sh](https://www.christophe-casalegno.com/tools/brainfw.sh)

Amusez-vous bien.