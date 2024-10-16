# Report Week 4 - Group 09
You can find all of our related changes under [this fork repo](https://github.com/mrdedede/Chess).

## André FILHO

Our tasks for this week were rather simple, basically we should run mutation tests under our Chess tests, which was not complicated to run, so I actually took a few steps before and after running it, here we can find the steps taken:

1. Get the test coverage so far
2. Get the mutation score for our tests so far
3. Analyse the results of the mutation tests
4. Implement new tests based on the mutation results
5. Run test coverage and mutation score analysis

### Test Coverage

After running the test coverage analysis with Dr. Test in Pharo's Browser, we got some interesting results:
- 53.69% of Coverage for our test suite
- 69 uncovered methods
- 9 partially covered methods

This analysis has shown us that some further work on coverage is needed, especially for tests on methods that are not covered for now.

### Get mutation test analysis / score

Taking the following approach:
```smalltalk
testCases := { MyKingTest. MyRookTests. MyBishopTests }.
classesToMutate := { MyKing. MyRook. MyBishop }.

analysis := MTAnalysis new
    testClasses: testCases;
    classesToMutate: classesToMutate.

analysis run.
analysis generalResult mutationScore.
```

We got 61% for our mutation score under this analysis, which is not a bad result per se, but indicates that there are indeed some tests we'd need to do to increase our coverage and test suit in general, before proceeding to implement more features.

### Analyse the results
With 83 surviving mutants and 139 killed mutants, besides the need of creating more test cases, when focusing more on the task at hand - making only the possible moves available at hand - I decided to have a look on some specific cases that are somewhat related to this functionality.

Especially the following ones:
```smalltalk
1. Replace #and with false in MyKing>>#isCheckMated
2. Replace #or with true in MyKing>>#targetSquaresLegal:
3. Remove ^ in  MyKing>>#isCheckMated
4. Replace #and with #or in MyKing>>#targetSquaresLegal
5. Replace #and with #or in MyKing>>#isCheckMated
6. Nullify the arguments of MyKing>>#targetSquaresLegal
```

After a brief look at the web for some references for what reference should we get when comparing Coverage and Mutation Ratio and how it relates to test quality. I've found [an article](https://arxiv.org/pdf/2309.02395) that promotes the view that, acutally a good indicator for test quality is the difference between both of these metrics, which still made me promptly think about ways in which we could close this gap, whilst trying to increase both of them simultaneosly - the most obvious answer for me, as to where we stand at this point, was the planning of further tests.

### Test Implementation

You can find the results of the implemented tests at [our fork for Chess](https://github.com/mrdedede/Chess).
To put it briefly, I added the tests which were either not covered or which the mutation under the king or the knight class were still alive and checked the next results, some example of test implementations were:

```smalltalk

testIsCheckMated

	| king board |
	board := MyChessBoard empty.
	board at: 'a1' put: (king := MyKing white).

	"Put two atacker rooks on e column"
	board at: 'e1' put: MyRook black.
	board at: 'e2' put: MyRook black.

	self assert: king isCheckMated 
```

Which asserted the king would be check-mated at those cases.

### Test Coverage / Mutation Analysis

After the implementations, we discovered some strange data:

1. Test coverage has been shown to actually decrease! By a lot! We're down from 53% and 69 uncovered methods to 33% and 99 uncovered methods. I am still not sure why this is. Maybe is there a bug under Pharo?
2. Mutation testing got a success of over 77%, which is a solid improvement after the previous result we've got, with only 51 living mutants.



# Salim TITOUCHE

Nos tâches pour cette semaine étaient plutôt simples, en gros comme l'a déjà expliqué mon camarade au-dessus, nous devions exécuter des tests de mutation sur nos tests d'échecs.

J'ai fait le même travail sur la classe MyPawn et les tests que j'ai créés, intitulés PawnMoveTests.

```smalltalk
testCases := { MyPawn }.
classesToMutate := { PawnMoveTests }.

analysis := MTAnalysis new
    testClasses: testCases;
    classesToMutate: classesToMutate.

analysis run.
analysis generalResult mutationScore.
```

J'ai obtenu le résultat suivant : 
- 44 % (26 mutants sauvés et 21 mutants tués
- 54.36% code Coverage 
- 68 uncovered methods
- 7 partially covered methods

Et pour améliorer ce résultat, je dois ajouter des tests pour tuer plus de mutants et couvrir plus de méthodes, et aussi corriger le code du jeu car déjà il n'est pas juste.

apré que jais ajouter dautre test jais optenu le resultas suivant : 
- 57 % (20 mutants sauvés et 27 mutants tués

### Code des tests

- [GitHub](https://github.com/mrdedede/Chess/blob/main/src/Myg-Chess-Tests/PawnMoveTests.class.st)


# Rapport Hebdomadaire Semaine 4 de Salas Merzouk

## Progrès

Cette semaine, j'ai continué à travailler sur le refactoring de la logique du jeu d'échecs, en me concentrant particulièrement sur l'application des tests de mutation pour évaluer et améliorer la qualité de mes tests unitaires.

### Tâches réalisées :

1. **Application des tests de mutation sur MyChessSquare**

J'ai exécuté des tests de mutation sur la classe MyChessSquare pour évaluer la robustesse de ma suite de tests. Les résultats initiaux ont révélé un score de mutation de 11%, ce qui est relativement bas. Cependant, ce résultat n'est pas surprenant étant donné que j'ai actuellement 30 classes mais seulement 4 tests implémentés.

2. **Analyse des résultats des tests de mutation**

Le faible score de mutation m'a permis de tirer plusieurs enseignements :

- **Couverture insuffisante** : Mes tests actuels ne couvrent qu'une petite partie des scénarios possibles et des comportements de la classe.
- **Nécessité d'expansion** : Il est clair que je dois augmenter considérablement le nombre et la diversité de mes tests pour améliorer la robustesse de mon code.
- **Identification des points faibles** : Les tests de mutation ont mis en évidence des parties spécifiques du code qui ne sont pas suffisamment testées, me donnant ainsi des directions claires pour l'amélioration.

3. **Plan d'amélioration de la suite de tests**

Pour augmenter le score de mutation et améliorer la qualité globale de mes tests, je dois :

- Augmenter le nombre de tests pour couvrir plus de scénarios.
- Diversifier les tests pour inclure plus de cas exceptionnelles.
- Ajouter des tests spécifiques pour chaque méthode de la classe MyChessSquare.

## Réflexions et apprentissages

L'application des tests de mutation a été une expérience très instructive. Elle m'a permis de comprendre concrètement l'importance d'une suite de tests complète et diversifiée. J'ai réalisé que mes tests initiaux, bien que fonctionnels, ne couvraient qu'une fraction des cas possibles.

Cette expérience m'a également montré l'utilité des tests de mutation comme outil pour évaluer et améliorer la qualité des tests unitaires. C'est un moyen efficace de découvrir les faiblesses de notre suite de tests et de guider nos efforts d'amélioration.


