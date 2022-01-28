Opis ogólny
===========

Mechanizm powinien umożliwiać walidację parametrów logik, pluginów i
efektów na etapie dodawania kluczy.

Constrainty powinny być definiowane przez osoby piszące pluginy. (Być
może jakiś delegat w modelu itp.) Przy walidacji musi być możliwość
uwzględnienia jakiegoś szerszego kontekstu, czyli np:

-   innych parametrów
-   sąsiadujących kluczy
-   zastosowanych typów interpolacji (być może konieczne jest
    zaimplementowanie mechanizmu, który pozwala odpytać interpolator o
    zakres zwracanych wartości przy jakichś kluczach)

Przykładowe problemy
--------------------

-   Użytkownik dodaje dwa sąsiadujące klucze, które są poprawne, jednak
    podczas interpolacji, mogą powstać niepoprawne wartości pośrednie.
-   Id maski i id noda z efektu NodeMask, są od siebie zależne, ponieważ
    nie powinny wskazywać na ten sam node. Tu pojawia się dodatkowy
    problem samej zmiany parametrów,

jeżeli są tylko dwa nody. Jeżeli nie zmieni się ich jednocześnie, to nie
da się ich zmienić w ogóle, bo zawsze będą odpadały na constraincie.
Obecnie mechanizmy silnika nie pozwalają na jednoczesną modyfikację.

-   Parametry pluginów geometrycznych są od siebie zależne. Np. suma
    beveli (górnego i dolnego) nie powinna być większa niż wysokość
    geometri