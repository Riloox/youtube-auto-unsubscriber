### `README.md`

# Script de Anulaci贸n Masiva de Suscripciones en YouTube

Este script en JavaScript permite anular suscripciones de manera masiva en YouTube mediante la manipulaci贸n del DOM en el navegador. Es ideal para limpiar tu lista de suscripciones de forma r谩pida y eficiente.

## 馃殌 Caracter铆sticas

- Anula suscripciones de canales de YouTube de manera automatizada.
- Registra el progreso, incluyendo los canales procesados, los 茅xitos y los fallos.
- A帽ade retrasos entre acciones para evitar conflictos con la interfaz de YouTube.

## 馃洜 Requisitos

- Un navegador web compatible con la ejecuci贸n de scripts en la consola del desarrollador.
- Acceso a tu cuenta de YouTube en el navegador.

## 馃搵 Instrucciones de Uso

1. Abre YouTube en tu navegador y accede a la p谩gina de suscripciones de canales.
2. Presiona `F12` o `Ctrl+Shift+I` (Windows/Linux) o `Cmd+Option+I` (Mac) para abrir las herramientas de desarrollador.
3. Ve a la pesta帽a **Consola**.
4. Copia y pega el c贸digo del script en la consola.
```
async function unsubscribeFromChannel(channelRow) {
    try {
        // Find the subscribe button within this channel's row
        const subscribeButton = channelRow.querySelector('ytd-subscribe-button-renderer button');
        if (!subscribeButton) {
            console.log('Bot贸n de suscripci贸n no encontrado');
            return false;
        }

        // Click the subscribe button to open the dialog
        subscribeButton.click();
        await new Promise(resolve => setTimeout(resolve, 1000));

        // Find and click the "Anular suscripci贸n" link in the dialog
        const unsubscribeButton = Array.from(document.querySelectorAll('yt-button-renderer a'))
            .find(link => link.textContent.trim() === 'Anular suscripci贸n');
        
        if (!unsubscribeButton) {
            // Try alternative selector if the first one fails
            const altUnsubscribeButton = document.querySelector('button[aria-label="Anular suscripci贸n"]');
            if (!altUnsubscribeButton) {
                console.log('Bot贸n de anular suscripci贸n no encontrado');
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
            console.log(`鉁?Anulada suscripci贸n de: ${channelName}`);
        } else {
            failCount++;
            console.log(`鉂?Error al anular suscripci贸n de: ${channelName}`);
        }
        
        // Wait between unsubscriptions to avoid overwhelming YouTube
        await new Promise(resolve => setTimeout(resolve, 2000));
    }
    
    console.log(`
    =================
    Proceso Completado
    =================
    Suscripciones anuladas con 茅xito: ${successCount}
    Fallos: ${failCount}
    Total procesados: ${totalChannels}
    `);
}

// Start the unsubscription process
console.log('Iniciando proceso de anulaci贸n masiva de suscripciones...');
massUnsubscribe().catch(error => console.error('Error fatal:', error));
```
5. Presiona `Enter` para iniciar el proceso.

El script buscar谩 autom谩ticamente todos los canales en la p谩gina y anular谩 suscripciones de manera secuencial.

## 馃搶 Notas

- **Retrasos entre acciones:** El script incluye pausas para evitar que se marque como actividad sospechosa.
- **Mensajes en consola:** El progreso del script se muestra en la consola, incluyendo los canales procesados, 茅xitos y fallos.
- **Limitaciones:** Si tienes demasiadas suscripciones, puede ser necesario desplazarte hacia abajo en la p谩gina para cargar todos los canales antes de ejecutar el script.

## 鈿狅笍 Advertencias

- Este script interact煤a con el DOM de YouTube y podr铆a dejar de funcionar si se realizan cambios en la estructura de la p谩gina.
- 脷salo bajo tu propia responsabilidad. Este tipo de manipulaci贸n puede no cumplir con los t茅rminos de servicio de YouTube.
