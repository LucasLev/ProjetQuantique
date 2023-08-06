# MTI882 Devoir 2

Lucas Lévesque - Été 2023

## Circuits

1. En utilisant la table de vérité du CNOT, on sait que $|0\rang \otimes |0\rang$ reste $|0\rang \otimes |0\rang$ (on flip le second bit uniquement si le premier est dans l'état $|1\rang$). Le passage d'un $|0\rang$ dans une porte de Hadamard donne l'état $|+\rang$, donc on obtiens l'état $|+\rang \otimes |+\rang$.
2. Si le premier qubit est $|-\rang$, alors la première porte de Hadamard va faire basculer le circuit dans l'état $|1\rang \otimes |0\rang$. Cela va modifier l'état du CNOT, qui nous emmener dans l'état $|1\rang \otimes |1\rang$. Le passage par des portes Hadamard transforme la sortie en $|-\rang \otimes |-\rang$.
3. On pourrait en conclure que l'état du premier qubit est toujours reflété en sortie sur le deuxième.
4. Cet conclusion est invalidée par l'état initial $|+\rang \otimes |1\rang$. Cette configuration est très similaire à celle de la première simulation, mais dans celle ci les Hadamard finaux n'ont pas la même entrée : celui du premier qubit reçoit un $|0\rang$ tandis que celui du deuxième reçoit un $|1\rang$. L'état final est donc $|+\rang \otimes |-\rang$

## Portes de Clifford

1. Pour que $\mathcal{T}$ soit une porte de clifford, on doit avoir $\mathcal{T}\mathcal{X}\mathcal{T}^\dagger\in P^\dagger_1$ et $\mathcal{T}\mathcal{Z}\mathcal{T}^\dagger\in P^\dagger_1$. Or $$\mathcal{T}\mathcal{X}\mathcal{T}^\dagger = 
\begin{bmatrix}
   1 & 0 \\
   0 & e^{i\frac{\pi}{4}}
\end{bmatrix}
\begin{bmatrix}
   0 & 1 \\
   1 & 0
\end{bmatrix}
\begin{bmatrix}
   1 & 0 \\
   0 & e^{-i\frac{\pi}{4}}
\end{bmatrix} = \begin{bmatrix}
   0 & e^{-i\frac{\pi}{4}} \\
   e^{i\frac{\pi}{4}} & 0
\end{bmatrix} \notin P^\dagger_1
$$ Donc $\mathcal{T}$ n'est pas une porte de Clifford

2. Pour la porte Ctrl$\mathcal{S}$, on doit montrer que :
     - $\text{Ctrl}\mathcal{S} \cdot \mathcal{I}\otimes\mathcal{X}\cdot\text{Ctrl}\mathcal{S}^\dagger \in P^\dagger_2$
     - $\text{Ctrl}\mathcal{S} \cdot \mathcal{X}\otimes\mathcal{I}\cdot\text{Ctrl}\mathcal{S}^\dagger \in P^\dagger_2$
     - $\text{Ctrl}\mathcal{S} \cdot \mathcal{I}\otimes\mathcal{Z}\cdot\text{Ctrl}\mathcal{S}^\dagger \in P^\dagger_2$
     - $\text{Ctrl}\mathcal{S} \cdot \mathcal{Z}\otimes\mathcal{I}\cdot\text{Ctrl}\mathcal{S}^\dagger \in P^\dagger_2$
Tout d'abord, remarquons que comme $\mathcal{S}^\dagger = \mathcal{S}\mathcal{S}\mathcal{S}$, alors $\text{Ctrl}\mathcal{S}^\dagger = \text{Ctrl}\mathcal{S}\cdot\text{Ctrl}\mathcal{S}\cdot\text{Ctrl}\mathcal{S}$
Ainsi, on a $\text{Ctrl}\mathcal{S} \cdot \mathcal{I}\otimes\mathcal{X}\cdot\text{Ctrl}\mathcal{S}^\dagger = \text{Ctrl}\mathcal{S} \cdot \mathcal{I}\otimes\mathcal{X}\cdot\text{Ctrl}\mathcal{S}\cdot\text{Ctrl}\mathcal{S}\cdot\text{Ctrl}\mathcal{S}= \begin{pmatrix}0&1&0&0\\ 1&0&0&0\\ 0&0&0&-i\\ 0&0&i&0\end{pmatrix} \notin P^\dagger_2$
Donc $\text{Ctrl}\mathcal{S}$ n'est pas une porte de Clifford.

## Cryptographie

1. Pour lire des états $|+\rang$, il suffit à Bob d'ajouter ou non une porte de Hadamard juste avant la lecture : celle-ci va transformer les $|+\rang$ en $|0\rang$ et les $|-\rang$ en $|1\rang$
2. Passage en 3 bases
   1. Pour le tamisages, la probabilité pour Bob d'utiliser la même base que Alice est de $\frac{1}{3}$. Si Alice transmet $3n$ bits, alors seuls $n$ seront conservés après tamisage.
   2. Ève transmet une erreur uniquement si elle lit dans la mauvaise base. Comme Bob, elle possède $\frac{1}{3}$ de lire dans la bonne base. Ce qui signifie qu'elle transmet une erreur dans $1-\frac{1}{3}=\frac{2}{3}$ des cas.

## Algorithme de Grover

1. $f : \{0,1\}^4 \rarr \{0,1\}$
   1. Pour mettre en place cet oracle, on utilise une porte Ctrl$^4\mathcal{Z}$. Si les bits sont en configuration 0101, alors la porte s'applique : ![Circuit montrant l'oracle de la fonction f](image.png). Les portes H ici ajoutée sont pour transformé le Ctrl$^4\mathcal{X}$ en Ctrl$^4\mathcal{Z}$
   2. Pour chaque itération, l'algorithme de grover va changer le de chaque coefficient superposé de telle maniere : $2\lang a \rang - a_i$.
      1. Pour la première itération, toutes les valeurs superposées ont au début le même module soit : $\frac{1}{\sqrt{16}} = \frac{1}{4}$. Une fois le qubit cible flippé, on obtient $2\lang a \rang = \frac{2}{16} (\frac{15}{\sqrt{16}} - \frac{1}{\sqrt{16}}) = \frac{7}{16}$. Tous les bits passent donc à un module de $\frac{7}{16} - \frac{1}{\sqrt{16}} = \frac{3}{16}$ sauf le qubit cible qui passe à $\frac{7}{16} + \frac{1}{\sqrt{16}} = \frac{11}{16}$.
      2. Après la seconde itération :
         - $2\lang a \rang = \frac{2}{16} (\frac{15\times3}{16} - \frac{11}{16}) = \frac{17}{64}$
         - qubits cible : $\frac{17}{64} + \frac{11}{16} = \frac{61}{64}$
         - autres qubits : $\frac{17}{64} - \frac{3}{16} = \frac{5}{64}$
      3. Troisième itération :
         - $2\lang a \rang = \frac{2}{16} (\frac{15\times5}{64} - \frac{61}{64}) = \frac{7}{256}$
         - qubits cible : $\frac{7}{256} + \frac{61}{64} = \frac{251}{256}$
         - autres qubits : $\frac{7}{256} - \frac{5}{64} = -\frac{13}{256}$
Cette itération est la dernière dont on a besoin pour mesurer : le nombre optimal d'étapesp pour l'algorithme de grover est en effet $r \leq\frac{\pi}{4}\sqrt{2^n}$. Pour $n=4$, on obtient donc $\frac{\pi}{4}\sqrt{16} = \pi$, donc $r=3$
2. Oracle inversé
   1. Pour inverser un oracle, il suffit d'appliquer une porte $\mathcal{Z}$ sur l'un des bits de sa sortie : cela renverseras la phase de tous les bits, indiquant les marqués comme non marqués et inversement.
