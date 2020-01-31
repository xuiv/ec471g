boot to recovery
open terminal run: csrutil disable

cp -R -H AirPortAtheros40.kext /System/Library/Extensions/IO80211Family.kext/Contents/PlugIns/
cp -R -H ATH9KInjector.kext /System/Library/Extensions/
