# Opis

Odbicie środowiska przy użyciu podanej tekstury.

**Name**: environmental tex

**UID**: DEFAULT_ENVIRONMENTAL_TEXTURE

# Parametry

- **reflectivity **
- **envMat**
- **envMixMode**



# Przyszła implementacja

##Tryby mieszania materiału z teksturą

- **Blend**

    Interpolates between environment color and material. Uses **reflectivity** parameter.
    _Equation_: EnvColor * reflectivity + MaterialColor * (1.0 - reflectivity).

- **Decal**

    Uses only environment color and ignores material.

- **Modulate (or maybe multiply)**

    Multiplies environment color by material color. Ignores **reflectivity** parameter.

- **Add**

    Adds environment color to material color. Uses **reflectivity** parameter to attenuate environment.

- **Average**

    Adds material and environment color and devides by 2. Ignores **reflectivity** parameter.

- **Add Signed**

    Adds material and environment color and subtracts 0.5. Uses **reflectivity** parameter to attenuate environment.

##Przekształcenia na teksturze

Viz rzeczywiście wystawia również całą macierz transformacji, więc my zróbmy tak samo. Najwyżej w edytorze
można uniemożliwić edycję niektórych parametrów.