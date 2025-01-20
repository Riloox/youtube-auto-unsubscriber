### `README.md`

# Script de Anulación Masiva de Suscripciones en YouTube

Este script en JavaScript permite anular suscripciones de manera masiva en YouTube mediante la manipulación del DOM en el navegador. Es ideal para limpiar tu lista de suscripciones de forma rápida y eficiente.

## 🚀 Características

- Anula suscripciones de canales de YouTube de manera automatizada.
- Registra el progreso, incluyendo los canales procesados, los éxitos y los fallos.
- Añade retrasos entre acciones para evitar conflictos con la interfaz de YouTube.

## 🛠 Requisitos

- Un navegador web compatible con la ejecución de scripts en la consola del desarrollador.
- Acceso a tu cuenta de YouTube en el navegador.

## 📋 Instrucciones de Uso

1. Abre YouTube en tu navegador y accede a la página de suscripciones de canales.
2. Presiona `F12` o `Ctrl+Shift+I` (Windows/Linux) o `Cmd+Option+I` (Mac) para abrir las herramientas de desarrollador.
3. Ve a la pestaña **Consola**.
4. Copia y pega el código del script en la consola.
```
async function unsubscribeFromChannel(channelRow) {
    try {
        // Find the subscribe button within this channel's row
        const subscribeButton = channelRow.querySelector('ytd-subscribe-button-renderer button');
        if (!subscribeButton) {
            console.log('Botón de suscripción no encontrado');
            return false;
        }

        // Click the subscribe button to open the dialog
        subscribeButton.click();
        await new Promise(resolve => setTimeout(resolve, 1000));

        // Find and click the "Anular suscripción" link in the dialog
        const unsubscribeButton = Array.from(document.querySelectorAll('yt-button-renderer a'))
            .find(link => link.textContent.trim() === 'Anular suscripción');
        
        if (!unsubscribeButton) {
            // Try alternative selector if the first one fails
            const altUnsubscribeButton = document.querySelector('button[aria-label="Anular suscripción"]');
            if (!altUnsubscribeButton) {
                console.log('Botón de anular suscripción no encontrado');
                return false;
            }
            altUnsubscribeButton.click();
        } else {
            unsubscribeButton.click();
        }

        // Add a longer delay to ensure the unsubscribe action completes
        await new Promise(resolve => setTimeout(resolve, 2000));
        
        return true;
    } catch (error) {
        console.error('Error durante el proceso:', error);
        return false;
    }
}

async function massUnsubscribe() {
    // Find all channel rows
    const channelRows = document.querySelectorAll('ytd-channel-renderer');
    const totalChannels = channelRows.length;
    
    console.log(`Encontrados ${totalChannels} canales para procesar`);
    
    let successCount = 0;
    let failCount = 0;

    for (let i = 0; i < channelRows.length; i++) {
        const channelName = channelRows[i].querySelector('#channel-title')?.textContent?.trim() || 'Canal Desconocido';
        console.log(`Procesando ${i + 1}/${totalChannels}: ${channelName}`);
        
        const success = await unsubscribeFromChannel(channelRows[i]);
        
        if (success) {
            successCount++;
            console.log(`�?Anulada suscripción de: ${channelName}`);
        } else {
            failCount++;
            console.log(`�?Error al anular suscripción de: ${channelName}`);
        }
        
        // Wait between unsubscriptions to avoid overwhelming YouTube
        await new Promise(resolve => setTimeout(resolve, 2000));
    }
    
    console.log(`
    =================
    Proceso Completado
    =================
    Suscripciones anuladas con éxito: ${successCount}
    Fallos: ${failCount}
    Total procesados: ${totalChannels}
    `);
}

// Start the unsubscription process
console.log('Iniciando proceso de anulación masiva de suscripciones...');
massUnsubscribe().catch(error => console.error('Error fatal:', error));
```
5. Presiona `Enter` para iniciar el proceso.

El script buscará automáticamente todos los canales en la página y anulará suscripciones de manera secuencial.

## 📌 Notas

- **Retrasos entre acciones:** El script incluye pausas para evitar que se marque como actividad sospechosa.
- **Mensajes en consola:** El progreso del script se muestra en la consola, incluyendo los canales procesados, éxitos y fallos.
- **Limitaciones:** Si tienes demasiadas suscripciones, puede ser necesario desplazarte hacia abajo en la página para cargar todos los canales antes de ejecutar el script.

## ⚠️ Advertencias

- Este script interactúa con el DOM de YouTube y podría dejar de funcionar si se realizan cambios en la estructura de la página.
- Úsalo bajo tu propia responsabilidad. Este tipo de manipulación puede no cumplir con los términos de servicio de YouTube.
