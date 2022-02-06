
# La corrélation croisée - Mesure de similarité entre deux signaux
## Encadré par: Prof Ammour Alae
###  Objectifs
• La corrélation croisée entre deux signaux.

• Evaluer l’efficacité de cette mesure en présence du bruit.

## Mesure de similarité entre deux signaux

La **mesure de similarité**, par exemple **entre deux signaux**, consiste `a quantifier le degré de ressemblance **entre** ces **deux** séquences. Le degré de ressemblance, appelé également degré de simili- tude, est maximum quand **les deux signaux** `a comparer sont identiques.

1-On Charge dans l’espace de travail l’enregistrement correspondant en tapant la commande load('Ring.mat').

```Matlab
 clear all
 close all
 clc
load('Ring.mat');
```  


  

 
  
2-On plot ce signal en fonction du temps.

```Matlab 
Fs=44100;
Ts =1/Fs;
t =0:Ts:(length(y)-1)*Ts ;
 plot(t,y);
  %sound(y,Fs);
  ```
 3- En utilisant deux droites en pointillés rouges, on coupe le signal entre t=7s et t=8s (Commande : xline).

```Matlab 
 xline(7,'--r');
 xline(8,'--r');
 ```
 ![image](https://user-images.githubusercontent.com/85129301/152623806-ecba62f3-033e-4098-819e-e01f407d6413.png)


 4-On stock ce morceau dans la variable  « Fragment »,  et de meme  on le trace en fonction du temps:

```Matlab
 fragment=y(7*Fs:8*Fs);
 %sound(fragment,Fs);
 figure(2)
 plot(fragment)
 ```
 ![image](https://user-images.githubusercontent.com/85129301/152624052-3bddec63-5850-4a31-816b-e55ffa426f95.png)
 
 5- On calcule et on trace la corrélation croisée du signal complet et celle du  fragment.

```Matlab
  corr=xcorr(y,fragment);
  figure(3)
  plot(corr);
  %Le plus grand pic se produit à la valeur de décalage ou il y'a une correspondance entre les deux echantillions
  ```
  ![image](https://user-images.githubusercontent.com/85129301/152624469-6c428a91-d120-4f0f-8b60-62046dfa9460.png)

6- Prendre la valeur du décalage pour laquelle la corrélation entre les deux signaux est maximal, puis utilisez ce décalage pour faire apparaitre le fragment dans le signal. 

```Matlab
 figure(4);
 [m,d]=max(corr);      %find value and index of maximum value of cross-correlation amplitude
 delay=d-max(length(y),length(fragment));   %shift index d, as length(y)=2*N-1; where N is the length of the signals
 plot(y)                                     %Plot signal y
 hold on,plot([delay+1:length(fragment)+delay],fragment,'r');  
```
![image](https://user-images.githubusercontent.com/85129301/152624559-629b6687-79fd-42c0-b5f1-9ca1604786db.png)

## Mesure de similarité entre deux signaux en présence du bruit

Dans cette partie, nous chercherons à évaluer le degré de dépendance des deux signaux en présence du bruit, en utilisant toujours cette notion de corrélation croisée.

1- On ajoute un bruit blanc gaussien au signal de départ et aussi au fragment, puis on les trace  en fonction du temps. 

```Matlab 
 ynoise = awgn(y,30)+y;
 fnoise=awgn(fragment,30)+fragment;
 figure(5);
 plot(t,[y ynoise]);
 xline(7,'--r');
 xline(8,'--r');
 figure(6);
 plot(fnoise);
```
*signal de départ bruité:*

![image](https://user-images.githubusercontent.com/85129301/152624669-bdb33a58-a38f-4e5c-a942-fcbc121d6d11.png)

*fragment bruité:*

![image](https://user-images.githubusercontent.com/85129301/152624644-1dff02ff-a306-4eb1-b00c-349764eaed87.png)

2-On applique la fonction xcorr afin de calculer la corrélation croisée entre les deux signaux bruités, puis on évalue la capacité de cette mesure de détecter le fragment dans le signal en présence du bruit, puis on trace toutes les figures:

```Matlab 
corr1=xcorr(ynoise,fnoise);
  figure(7)
  plot(corr1);%Le plus grand pic se produit à la valeur de décalage lorsque les éléments de x et y correspondent exactement
  figure(8);
  [m,d]=max(corr1);      %find value and index of maximum value of cross-correlation amplitude
  delay=d-max(length(ynoise),length(fnoise));   %shift index d, as length(X1)=2*N-1; where N is the length of the signals
  plot(ynoise);
  hold on,plot([delay+1:length(fnoise)+delay],fnoise,'r');
```
voici la corrélation:

![image](https://user-images.githubusercontent.com/85129301/152624842-953d4f84-6ec4-4049-a4ac-aa8576c02025.png)

et voici le signal détecté sur le signal de départ:

![image](https://user-images.githubusercontent.com/85129301/152624820-826829a3-8f17-461f-8ad6-cbdec1fa7f42.png)


On Conclut que Le résultat de xcorr peut être interprété comme une estimation de la corrélation entre deux séquences aléatoires ou comme la corrélation déterministe entre deux signaux déterministes.
# ***FIN***
