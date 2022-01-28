Przykładowy efekt ExampleEffect - korzysta z FSE.

W pliku NodeEffect.h dodać NodeEffectType:

NET_EXAMPLE_EFFECT

W pliku NodeEffectFactoryu.cpp dodać wpis dla CreateNodeEffect():

case NodeEffectType::NET_EXAMPLE_EFFECT:
   return CreateExampleEffect();

NodeEffectPtr       CreateExampleEffect   ()
{
    return nullptr;
}

NodeEffect do dodania

Stepsy do dodania

FSE do dodania