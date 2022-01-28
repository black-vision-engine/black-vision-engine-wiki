Opis ogólny
===========

Param source to mechanizm, który pozwala na uzależnienie wartości
dowolnego parametru (pluginu, logiki, światła, efektu) od funkcji
wywołanej na innym parametrze bądź generatorze wartości (sinus, cosinus,
timeval, etc). Parametry mogą zależeć od kilku parametrów jednocześnie.
Każdy parametr źródłowy może być również zależny od innych parametrów.

Wartość parametru jest ustalana poprzez ewaluację parametrów źródłowych
i wyliczenie funkcji wywołanej dla pobranych wartości. Dla każdego
parametru źródłowego ewaluacja jest wykonywana dla czasu lokalnego
timelinu, do którego ten parametr został podłączony.

Założenia
=========

-   ParamSource jest niezależnym od node-ów mechanizmem
-   Każda scena ma swoją własną kolekcje ParamSource, która poprawnie
    się (de)serializuje
-   Aby wykorzystać mechanizm ParamSource należy zgłosić event
    ParamSource z następującymi argumentami
    -   SceneName
    -   TargetType (light,plugin,logic,effect)
    -   TargetPath (nazwa światła bądź nodea np: "@light0" bądź \#0/\#0)
    -   TargetPluginName (nazwa pluginu / logiki / efektu bądź “light”
        dla światła)
    -   TargetParamName (nazwa paramu)
    -   TargetSubParamName (nazwa paramu) (np. X, Y , Z dla transforma)
    -   SourceType (light,plugin,logic,effect)
    -   SourcePath (nazwa światła bądź nodea np: "@light0" bądź \#0/\#0)
    -   SourcePluginName (nazwa pluginu / logiki / efektu bądź “light”
        dla światła)
    -   SourceParamName (nazwa paramu)
    -   SourceSubParamName (nazwa paramu) (np. X, Y , Z dla transforma)
-   Dodatkowo możemy określić
    -   Rodzaj mapowania parametrów
    -   Funkcję wywoływaną na ParamSource
    -   Parametry w/w funkcji

Dozwolone typy parametrów
=========================

-   ParamInt
-   ParamFloat
-   Vec2Float
-   Vec3Float
-   Vec4Float
-   String

Funkcje wywoływane na parametrach
=================================

po prawej stronie równania na SourceParamie możemy wywołać jedną z
funkcji operujących na SourceParamach:

-   Identity - zwraca parametr podany w argumencie, występuje w
    odmianach zgodnych z rodzajami parametrów
-   Multiply (float) - mnoży parametr przez podany współczynnik, w
    przypadku
-   GetTimeString(StringFormat np. “MM/dd/yy H:mm:ss” zgodnie z
    <https://msdn.microsoft.com/pl-pl/library/8kb3ddd4(v=vs.110).aspx>)

Domyślna funkcja wywoływana na SourceParamie

-   Identity

Domyślne mapowanie wartości parametrów
======================================

-   ParamInt
    -   ParamInt = ParamFloat
    -   ParamInt = ParamInt
    -   ParamInt = Vec2Float\[0\]
    -   ParamInt = Vec3Float\[0\]
    -   ParamInt = Vec4Float\[0\]
    -   ParamInt = String\_ToInt(String) // 0 jeśli niepoprawna
        konwersja
-   Float
    -   j/w w przypadku Int
-   Vec2Float
    -   Vec2Float\[0\] = ParamInt \_ \_ \_ \_ \_ \_ \_ \_ Vec2Float\[1\]
        = OLD\_VALUE
    -   Vec2Float\[0\] = ParamFloat \_ \_ \_ \_ \_ \_ \_ \_
        Vec2Float\[1\] = OLD\_VALUE
    -   Vec2FloatA\[0\] = Vec2FloatB\[0\] \_ \_ \_ \_ \_ \_ \_ \_
        Vec2FloatA\[1\] = Vec2FloatB\[1\]
    -   Vec2FloatA\[0\] = Vec3FloatB\[0\] \_ \_ \_ \_ \_ \_ \_ \_
        Vec2FloatA\[1\] = Vec3FloatB\[1\]
    -   Vec2FloatA\[0\] = Vec4FloatB\[0\] \_ \_ \_ \_ \_ \_ \_ \_
        Vec2FloatA\[1\] = Vec4FloatB\[1\]
    -   Vec2FloatA\[0\] = String\_ToFloat(String) \_ \_ \_ \_ \_ \_ \_
        \_ Vec2FloatA\[1\] = OLD\_VALUE
-   Vec3Float
    -   j/w w przypadku Vec2Float
-   Vec4Float
    -   j/w w przypadku Vec2Float
-   String = ParamSource.ToString()

Rodzaje mapowania wartości parametrów
=====================================

Możemy ustawić indeksy parametrów które mówią o tym która pozycja
wektora jest mapowana na którą

Np mapując Stosując mapowanie ParamVec3 = ParamVec2 Func(ParamVec2) oraz
ustalając TargetIndex = \[1,-1,0\] sprawimy, że ParamVec3\[0\] =
Func(ParamVec2)\[1\] ParamVec3\[1\] = OLD\_VALUE ParamVec3\[2\] =
Func(ParamVec2)\[0\]

Generatory funkcji
==================

-   float sinus(skwantowany globalny czas aplikacji \* parametr)
-   float cosinus(skwantowany globalny czas aplikacji \* parametr)
-   int obecna\_godzina24()
-   int obecna\_godzina12()
-   int obecna\_minuta()
-   int obecna\_sekunda()
-   int obecna\_ms()
-   int sekunda\_od\_uruchomienia\_silnika()

Składarka funkcji
=================

Względem tego

Parametry tylko do odczytu
==========================

Potrzebny nowy typ parametru (QueryParam czy coś w tym rodzaju). Obiekt,
który posiada taki parametr, sam ustawia jego wartość. Taki parametr
może być odczytany przez edytor, albo może służyć jako źródło dla param
source.

Przykładowym use casem są bounding boxy, których rozmiary są potrzebne,
żeby do nich wyrównywać inne obiekty przy pomocy param sourców.