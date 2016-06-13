﻿---
title: ALSourceb
---

# ALSourceb
_namespace: [OpenTK.Audio.OpenAL](N-OpenTK.Audio.OpenAL.html)_

A list of valid 8-bit boolean Source/GetSource parameters



### Properties

#### EfxAuxiliarySendFilterGainAuto
(EFX Extension) If this Source property is set to True, the intensity of this Source’s reflected sound is automatically attenuated according to source-listener distance and source directivity (as determined by the cone parameters). If it is False, the reflected sound is not attenuated according to distance and directivity. Type: bool Range [False, True] Default: True
#### EfxAuxiliarySendFilterGainHighFrequencyAuto
(EFX Extension) If this Source property is AL_TRUE (its default value), the intensity of this Source’s reflected sound at high frequencies will be automatically attenuated according to the high-frequency source directivity as set by the Sourcef.ConeOuterGainHF property. If this property is AL_FALSE, the Source’s reflected sound is not filtered at all according to the Source’s directivity. Type: bool Range [False, True] Default: True
#### EfxDirectFilterGainHighFrequencyAuto
(EFX Extension) If this Source property is set to True, this Source’s direct-path is automatically filtered according to the orientation of the source relative to the listener and the setting of the Source property Sourcef.ConeOuterGainHF. Type: bool Range [False, True] Default: True
#### Looping
Indicate whether the Source is looping. Type: bool Range: [True, False] Default: False.
#### SourceRelative
Indicate that the Source has relative coordinates. Type: bool Range: [True, False]

