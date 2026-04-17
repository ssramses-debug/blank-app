# Dashboard de Cobertura Nacional

Este repositorio incluye dos formas de ejecutar la aplicacion:

1. Version web con Streamlit (`streamlit_app.py`).
2. App Android instalable en `android_app/` (APK debug).

## Ejecutar la version web

1. Instala dependencias:
   - `pip install -r requirements.txt`
2. Inicia Streamlit:
   - `streamlit run streamlit_app.py`

## Compilar APK Android

Desde la carpeta `android_app/`:

1. Configura un Android SDK local (si no existe `local.properties`):
   - `echo "sdk.dir=/ruta/a/tu/android-sdk" > local.properties`
2. Compila APK debug:
   - `./gradlew assembleDebug`
3. APK generado en:
   - `android_app/app/build/outputs/apk/debug/app-debug.apk`
