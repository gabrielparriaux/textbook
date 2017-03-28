
La montée des réseaux informatiques dans les systèmes d’informations a été phénoménale. Il y a moins de 20 ans, la plupart des ordinateurs n’étaient pas connectés à un réseau. En fait, l’idée même d’«ordinateur personnel» (_personal computer_ ou PC) était que l’ordinateur était personnel, et donc qu’il n’était pas partagé avec d’autres personnes, et par conséquent qu’il n’était pas connecté avec d’autres ordinateurs. L’avènement d’Internet et du _World Wide Web_ a radicalement changé cette notion. Virtuellement, tous les ordinateurs sont devenus connectés à Internet. Les réseaux informatiques ont ensuite connu une autre avancée avec l’arrivée des téléphones portables connectés, les smartphones. La disponibilité générale des réseaux sans fils, destinés à l’origine aux communications vocales mais rapidement adaptés aux communications de données, a ajouté une quantité énorme «d’ordinateurs» à Internet.

Nous allons commencer ce chapitre avec deux notions importantes dans les réseaux informatiques: les protocoles et les paquets. Nous développerons un protocole simple de transmission de paquets pour illustrer la notion d’en-tête de paquet. Après cela, nous regarderons Internet, et nous introduirons la notion de réseau en mode différé (_store-and-forward_). La communication entre deux machines dans un tel réseau donne lieu à des protocoles extrêmement complexes. Nous introduirons les notions de modularité et de couches réseaux pour gérer cette complexité. Comme exemple de protocole utilisant des couches, nous introduirons la famille de protocole TCP/IP qui est utilisé sur Internet. Finalement, nous regarderons de plus prêt le protocole IP de la famille TCP/IP et son protocole de routage.


# La notion de protocole #

La notion centrale dans les réseaux informatiques est la notion de protocole.
@{startObjective: Expliquer la définition du concept de «protocole»}
Un protocole est, en général, un ensemble de règles qui régissent la conduite d’une conversation entre un certain nombre de parties.
@{endObjective: Expliquer la définition du concept de «protocole»}
Le principe des protocoles est bien entendu connu en dehors de la communication des ordinateurs.

Les protocoles existent en communication entre les êtres humains. Ils peuvent être très informels ou au contraire être très formels, selon les standards des personnes impliquées et les circonstances de la communication. Par exemple, des amis peuvent suivre un protocole très informel, en s’interrompant les uns les autres librement lorsque quelqu’un a quelque chose à dire. Dans une salle de classe par contre, les règles sont plus strictes. Normalement le professeur parle, et les étudiants écoutent, levant la main lorsqu’ils veulent dire quelque chose.



L’exemple précédent ne s’intéresse qu’à un seul aspect d’un protocole de communication: qui peut parler à un moment donné. De nombreux autres aspects doivent aussi être convenus. Par exemple, il doit y avoir un moyen pour signifier que la communication commence et qu’elle se termine. Typiquement, un silence indique qu’une communication est finie. Dans une conversation entre plusieurs participants, il y a souvent un protocole pour indiquer à qui une communication s’adresse. Il faut aussi un moyen de corriger les erreurs ou les perturbations du canal de communication, le plus simple étant de demander à la personne qui parle de répéter ce qu’elle vient de dire.
 Il y a aussi de nombreuses règles implicites dans une communication entre plusieurs humains. Par exemple, il y a la langue dans laquelle la conversation se tient. Si les personnes ne se connaissent pas, il peut y avoir une partie d’initiation du protocole dans laquelle elles établissent quelle langage elles ont en commun, et, s’il y en a plus d’un, lequel elles vont utiliser.

La plupart de ces problèmes se retrouvent dans les communications entre ordinateurs. Par exemple, il faut une règle pour établir quel ordinateur a le droit d’utiliser le canal de communication à quel moment. S’il y a plus de deux machines sur le canal, il faut un moyen pour s’adresser à une machine particulière. S’il y a du bruit sur le canal, alors il faut pouvoir s’assurer que ce qui a été reçu est identique à ce qui a été envoyé, et de plus, il faut souvent que celui qui envoie puisse s’assurer que le receveur a bien reçu ce qu’il lui a envoyé.

Quelles que soient les similitudes qui existent entre les communications entre humains ou entre ordinateurs, il y a aussi une énorme différence. Les humains sont tout à fait capables de gérer des situations informelles et parfois ambiguës. Les ordinateurs sont très mauvais pour cela. Résultat, les protocoles de conversations entre humains peuvent sembler très formels, mais sont pourtant incroyablement informels lorsqu’on les compare à ceux utilisés pour les communications entre ordinateurs. Dans les premiers, beaucoup d’aspects ne sont pas définis ou ne le sont pas formellement, et les ambiguïtés sont rapidement résolues grâce à l’intelligence humaine. Dans les seconds, chaque petit détail doit être précisément défini, parce que toute ambiguïté pourrait causer un échec de la communication. Nous allons maintenant regarder quelques exemples simples de protocoles de communications entre ordinateurs.


# Un exemple de protocole #

Nous allons commencer par illustrer la notion de protocole de communication entre ordinateurs avec quelques exemples simples. Ensuite, nous définirons à partir de ces exemples des notions simples sur la construction de protocole. Pour finir nous regarderons un ensemble spécifique de protocoles, ceux utilisés sur Internet.

@{startObjective: Expliquer sous quelle forme sont transmises les informations numériques sur un réseau}
Dans tous ces exemples nous supposons qu’une machine connectée à un réseau peut envoyer un paquet sur ce réseau ou recevoir un paquet du réseau.
@{endObjective: Expliquer sous quelle forme sont transmises les informations numériques sur un réseau}
@{startObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}
Un paquet a un marqueur de début et un marqueur de fin pour signaler respectivement son début et sa fin.
@{endObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}

@{startObjective: Identifier les problématiques principales de l’envoi de données sur un réseau}
En général, il n’est pas garanti qu’un paquet envoyé sur le réseau est en fait bien reçu. De plus, même si un paquet est bien reçu par le destinataire, il n’est pas garanti non plus que le paquet reçu est identique à celui qui a été envoyé. Ainsi, même si avec forte probabilité, un paquet envoyé est aussi reçu et le paquet reçu est identique au paquet envoyé, cela n’est pas garanti à 100%. De nombreuses mésaventures peuvent perturber la bonne réception d’un paquet. Par exemple, le lien de communication entre celui qui envoie et celui qui reçoit peut avoir été coupé, ou il peut être temporairement perturbé rendant ainsi la sortie différente de l’entrée.
@{endObjective: Identifier les problématiques principales de l’envoi de données sur un réseau}
Comment pouvons-nous établir une vraie communication entre celui qui envoie et celui qui reçoit avec de tels scénarios pouvant arriver? C’est ce que nous allons voir dans les exemples qui arrivent.

   @ex.defn(label = 'exemple1)

Dans notre premier exemple, nos objectifs sont modestes. Nous voulons simplement que le receveur puisse déterminer si le paquet reçu est bien identique à celui qui a été envoyé.

@{startObjective: Expliquer le rôle d’une somme de contrôle ainsi que son fonctionnement}
Pour atteindre cet objectif, nous ajoutons ce qu’on appelle une _somme de contrôle_ (_checksum_) à chaque paquet. La somme de contrôle est une information redondante, c’est-à-dire qu’elle n’ajoute pas de nouvelle information au paquet, mais elle permet au destinataire de détecter si les données du paquet ont été modifiées pendant la transmission. L’idée est très simple: l’expéditeur calcule la somme de contrôle en exécutant un certain algorithme (déterminé avec le destinataire) sur les données qu’il veut envoyer. L’expéditeur envoie ensuite les données et la somme de contrôle. Le destinataire peut ensuite appliquer le même algorithme aux données, et vérifier qu’il obtient bien le même résultat que la somme de contrôle arrivée dans le paquet.
@{endObjective: Expliquer le rôle d’une somme de contrôle ainsi que son fonctionnement}
@{startObjective: Rappeler la distinction entre «paquet accepté» et «paquet rejeté»}
Si c’est le cas, le paquet est accepté, sinon il est rejeté.
@{endObjective: Rappeler la distinction entre «paquet accepté» et «paquet rejeté»}

Un exemple très simple de somme de contrôle consiste à calculer le _ou exclusif_ de tous les bits des données. Ce type de somme de contrôle est en fait assez faible. Il peut détecter une erreur d’un seul bit, mais il ne peut pas détecter une situation où, par exemple, deux bits auraient été échangés. Il existe des techniques plus sophistiquées de somme de contrôle, qui peuvent détecter une grande variété d’erreurs avec très grande probabilité. Une technique bien connue et beaucoup utilisée est celle du contrôle de redondance cyclique (_cyclic redundancy checksum_ ou CRC). Cette méthode permet non seulement de détecter beaucoup d’erreurs, mais en plus elle se prête bien à une implémentation matérielle, ce qui la rend très efficace.

Dans le reste de ce chapitre nous supposerons que chaque paquet contient une somme de contrôle, et que les destinataires les vérifient systématiquement, nous ne la mentionnerons donc plus.

@{startExercise: TODO-Titre (11.12.D)}

La page <http://dvsoft.developpez.com/Articles/CRC/#L8> permet de charger un programme de calcul de checksum (cf point 9). Téléchargez ce programme, puis sélectionnez, dans l’onglet `Type de calcul`, le programme `Checksum TCP/IP`. Le checksum sera un nombre hexadécimal (base 16) de 8 chiffres (donc un checksum de 32 bits). Ensuite:

 1. Entrez une chaîne de caractère quelconque et lancez le calcul du CRC en sélectionnant le menu `Calculer`.
 2. Modifiez légèrement la chaîne de caractère et relancez le calcul du CRC. Qu’observez-vous?
Essayez de modifier la chaîne de caractère de telle manière qu’elle soit différente de la chaîne entrée sous 1), mais donne le même CRC que sous 1).

@{endExercise}

@{startExercise: Bande passante et latence (11.12.1)}

 La bande passante d’un réseau est le nombre de bits qui peuvent être envoyés, exprimé habituellement en mégabits (10<sup>6</sup> bits) par seconde (Mbps) ou gigabits (10<sup>9</sup> bits) par seconde (Gbps). La latence d’un réseau est le temps nécessaire à un bit pour aller d’une machine à une autre. Avec ces définitions, répondre aux questions suivantes.

 Les machines A et B sont connectées par un réseau Ethernet à 1 Gbps. Elles sont dans la même pièce, et donc la latence est très faible, disons 1 microseconde (10<sup>-6</sup> seconde). La machine A envoie un paquet de 1000 octets à la machine B. La machine A commence à transmettre à l’instant 0.

 1. À quel instant le premier bit du paquet arrive-t-il à B?
 2. À quel instant le dernier bit du paquet arrive-t-il à B?

@{endExercise}

@{startExercise: Bande passante et latence (11.12.2)}

Considérons le même problème, avec cette fois un réseau satellitaire entre les machines A et B. La bande passante est de 1 Mbps et la latence de 540 millisecondes (le temps nécessaire à un signal pour aller de la terre jusqu’à un satellite géostationnaire et retour).

 1. À quel instant le premier bit du paquet arrive-t-il à B?
 2. À quel instant le dernier bit du paquet arrive-t-il à B?

@{endExercise}


@{startExercise: Bande passante et latence (11.12.1)}

 La bande passante d’un réseau est le nombre de bits qui peuvent être envoyés, exprimé habituellement en mégabits (10<sup>6</sup> bits) par seconde (Mbps) ou gigabits (10<sup>9</sup> bits) par seconde (Gbps). La latence d’un réseau est le temps nécessaire à un bit pour aller d’une machine à une autre. Avec ces définitions, répondre aux questions suivantes.

 Les machines A et B sont connectées par un réseau Ethernet à 1 Gbps. Elles sont dans la même pièce, et donc la latence est très faible, disons 1 microseconde (10<sup>-6</sup> seconde). La machine A envoie un paquet de 1000 octets à la machine B. La machine A commence à transmettre à l’instant 0.

 1. À quel instant le premier bit du paquet arrive-t-il à B?
 2. À quel instant le dernier bit du paquet arrive-t-il à B?

@{endExercise}

@{startExercise: Bande passante et latence (11.12.2)}

Considérons le même problème, avec cette fois un réseau satellitaire entre les machines A et B. La bande passante est de 1 Mbps et la latence de 540 millisecondes (le temps nécessaire à un signal pour aller de la terre jusqu’à un satellite géostationnaire et retour).

 1. À quel instant le premier bit du paquet arrive-t-il à B?
 2. À quel instant le dernier bit du paquet arrive-t-il à B?

@{endExercise}


@ex.defn(label = 'exemple2)

@{startObjective: Expliquer le rôle d’un «accusé de réception»}
Maintenant que nous avons déterminé que le destinataire peut vérifier que ce qu’il a reçu est identique à ce qui a été envoyé, nous allons aller un peu plus loin et ajouter un objectif: l’expéditeur veut savoir si le paquet envoyé a bien été reçu par le destinataire.

Ce deuxième protocole repose sur la notion d’_accusé de réception_. Lorsque le destinataire reçoit un paquet et vérifie qu’il est correct, il renvoie un accusé de réception à l’expéditeur, disant «je l’ai bien reçu». Maintenant l’expéditeur peut être sûr que le paquet qu’il a envoyé a bien été reçu.
@{endObjective: Expliquer le rôle d’un «accusé de réception»}

@ex.defn(label = 'exemple3)

Rendons maintenant les choses un petit peu plus intéressantes en disant que A et B s’envoient tous les deux des paquets. Cette situation pose une nouvelle question importante:
@{startObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}
comment A va-t-il distinguer si un paquet reçu de B est un nouveau paquet envoyé par B ou si c’est un accusé de réception d’un paquet qu’il a envoyé à B précédemment?

La solution pour répondre à ce problème consiste à ajouter dans les paquets un _type_, disant si le paquet contient des données ou si le paquet contient un accusé de réception (voir @fig.fullref('figure1)). Ces champs de type dans les paquets ont en fait beaucoup d’usages possibles et sont universels dans la communication des ordinateurs.
@{endObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}


   @fig.defn(label = 'figure1, caption = "Un paquet contenant des données et un champ de type")


@{startExercise: Ajout d’un accusé de réception  (11.12.3)}

Nous étendons maintenant la situation des deux exercices précédents au protocole de transmission fiable décrit dans l’exemple 11.2. Rappelons que le protocole consiste à envoyer un paquet de données de A à B, suivi de l’envoi d’un accusé de réception de B à A.

Un accusé de réception est typiquement court, disons 10 octets (y compris l’en-tête). La taille du paquet de données est toujours de 1000 octets (y compris l’en-tête).

 1. À quel instant le dernier bit de l’accusé de réception arrive-t-il à la machine A, en considérons la situation de l’exercice @ex.ref(11.12.1)?
 2. À quel instant le dernier bit de l’accusé de réception arrive-t-il à la machine A, en considérons la situation de l’exercice @ex.ref(11.12.2)?

@{endExercise}


    @ex.defn(label = 'exemple4)

Que va faire A s’il ne reçoit pas d’accusé de réception de B, mais s’il veut quand même être sûr que B a bien reçu son paquet? Une possibilité est de _retransmettre_ le paquet. A peut le faire plusieurs fois, jusqu’à ce qu’il reçoive un accusé de réception ou qu’il abandonne.

Cette stratégie apparemment innocente cause un petit problème. Supposons que le paquet original envoyé à B par A soit en fait arrivé, mais que le paquet accusé de réception de B à A ne soit pas arrivé. Maintenant A retransmet, et le paquet retransmis arrive à B. Comment B sait-il que c’est un duplicata causé par une retransmission et non un paquet original?

@{startObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}
Pour gérer cette situation, un paquet envoyé par A à B contient un _numéro de séquence_ (voir @fig.fullref('figure2)). Si ce paquet est retransmis, le numéro de séquence va rester le même.
@{endObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}
Si B se souvient quels numéros de séquence il a déjà reçus, il peut maintenant identifier les duplicatas. Il est important de noter que, quand B reçoit un duplicata, il doit quand même envoyer un accusé de réception, puisque si A a envoyé un duplicata, c’est probablement parce qu’il n’avait pas reçu le premier accusé de réception.


    @fig.defn(label = 'figure2, caption = "Un paquet composé d’un champ de type, d’un numéro de séquence et des données")

    @ex.defn(label = 'exemple5)

Maintenant, dans un cas plus général, A peut vouloir envoyer à B plus d’un seul paquet, et donc une séquence de paquets. Par exemple, si A veut envoyer un fichier à B, il va diviser le fichier en morceaux qui font la taille des paquets, et envoyer ces morceaux un paquet à la fois. Pour régler un cas comme celui-là, nous incrémentons simplement le numéro de séquence par un à chaque paquet successif envoyé par A. De la même façon, l’accusé de réception contient le numéro de séquence du paquet reçu.


@{startExercise: Ajout d’un accusé de réception (11.12.4)}

Regardons maintenant plus en détails la situation de l’exercice @ex.ref(11.12.3}, plus particulièrement dans le cas du réseau satellitaire. Rappelons que la machine A ne transmet le paquet suivant qu’après avoir reçu l’accusé de réception du paquet courant. Voyez-vous un problème?  Si oui, lequel? Comment peut-il être résolu avec une légère modification du protocole?

@{endExercise}


@{startExercise: TODO-Titre (11.12.C)}

On considère deux machines A et B.

 1. La machine A envoie un paquet de données m1 à B, l’un en t=0. Un paquet met deux unités de temps pour arriver à B. Si B reçoit un paquet correct à l’instant t, il envoie un accusé de réception en t+1. A l’aide d’un diagramme temporel, illustrer les trois scénarios suivants:
     * Tous les paquets sont bien reçus.
     * Tous les paquets sont bien reçus, sauf le premier accusé de réception.
     * Tous les paquets sont bien reçus, sauf le premier paquet de données.
   Mentionner le « type » et le « numéro de séquence » de chaque paquet.
 2. Même question, en supposant cette fois deux paquets m1 et m2 envoyés par A à B, m1 envoyé à l’instant t=0 et m2 à l’instant t=1. Considérer les trois mêmes scénarios.

**NB:** Un exemple de diagramme temporel est donné par la figure ci-dessous. Le temps s’écoule vers la droite. A l’instant t1 la machine A envoie le paquet p1 à la machine B. Le paquet est reçu à l’instant t2. Plus tard, à l’instant t3, la machine B envoie le paquet p2 à la machine A. Le paquet est reçu à l’instant t4. Bien évidement on a t1 < t2 < t3 < t4.

    @fig.defn(TODO diagramme temporel)

@{endExercise}


    @ex.defn(label = 'exemple6)

Nous allons compliquer une dernière fois notre exemple. Disons maintenant qu’il n’y a plus deux ordinateurs dans le réseau mais beaucoup plus.
@{startObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}
Nous avons donc besoin d’un mécanisme pour indiquer à quelle machine un paquet est destiné, pour que le destinataire sache qu’il est le destinataire. Nous devons alors aussi ajouter une indication pour dire qui est l’expéditeur, pour que l’accusé de réception puisse lui être retourné.

Pour cela, tous les ordinateurs du réseau doivent avoir une _adresse_ qui permet de les identifier de manière unique. Ainsi,il faut mettre dans chaque paquet l’adresse de la machine source et celle de la machine destinataire du paquet (voir @fig.fullref('figure3)). Si un paquet de données est envoyé de A à B, alors A est la source et B est la destination. Lorsque B envoie l’accusé de réception, alors B devient la source du paquet accusé de réception et A devient la destination.
@{endObjective: Décrire les principaux champs qui composent un en-tête de paquet ainsi que leur utilité}

   @fig.defn(label = 'figure3, caption = "Un paquet composé des adresses de la source et de la destination, d’un champ de type, d’un numéro de séquence et des données")

@{startObjective: Différencier les parties composant un «paquet»}
Ces exemples mettent en avant un des principes de base des réseaux informatiques. Un paquet est composé des données de ce paquet (parfois appelées la charge utile ou _payload_) et d’un en-tête donnant plus d’informations sur le paquet (voir @fig.fullref('figure4)).

    @fig.defn(label = 'figure4, caption = "La structure d’un paquet: un en-tête suivi des données")

L’en-tête des paquets donné dans l’exemple @ex.ref('exemple6) peut déjà sembler compliqué, mais en réalité les en-têtes de paquets sont bien plus compliqués que cela, et incluent des dizaines de champs différents, certains pouvant être optionnels.
@{endObjective: Différencier les parties composant un «paquet»}
Cela rend le logiciel sur les machines qui doit gérer le réseau très complexe. Pour réduire cette complexité, le traitement des paquets est fait de façon modulaire, en utilisant ce que l’on appelle des couches.
Nous étudierons les couches réseau et la modularisation plus tard dans ce chapitre, mais avant cela, nous allons regarder dans la section suivante Internet un petit peu plus en détails, puisqu’il a donné lieu à la couche TCP/IP universellement utilisée des protocoles réseaux.



