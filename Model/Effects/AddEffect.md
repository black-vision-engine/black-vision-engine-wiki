Model
-----

Silnik
------

Rozszerzenie listy dostępnych typów global efektów.
:   Dodać kolejne pole do enumeracji NodeEffectType w pliku
    Engine\\Graphics\\Effects\\NodeEffect\\NodeEffect.h
:   Uzupełnić ciało funkcji CreateNodeEffect w pliku.
    Engine\\Graphics\\Effects\\NodeEffect\\NodeEffectFactory.cpp
:   Tutaj tworzą się poszczególne efekty silnikowe. Wszystkie sś
    instacjami klasy bv::NodeEffect w odpowiednią wartością enuma w polu
    typ.
:   bv::NodeEffect zawiera klasę bv::NodeEffectLogic, która definiuję
    logikę nakładania efektu.
:   bv::NodeEffectLogic składa się z 3 różnych kroków renderowania.
:   1\. PreFullscreenEffectLogic
:   2\. FullscreenEffectInstance
:   3\. PostFullscreenEffectLogic
:   Pre i Post one są klasami abstrakcyjnymi. Je trzeba
    przeimplementować w celu zdefiniowania logiki efektu.
:   FullscreenEffectInstance trzyma context fullscreen efektu oraz
    instancję klasy abstarakcyjnej FullscreenEffect.
:   FullscreenEffect posiada dwie implementację. Jedna służąca do
    implementacji efektu składającego się z trywialnego grafu efektu z
    jednym nodem. Druga CompositeFullscreenEffect pozwalająca na
    budowanie grafów efektów.
:   wejścia i wyjścia renderowania pomiedzy nodami efektów w grafie
    definiowane są za pomocą FullscreenEffectContext.