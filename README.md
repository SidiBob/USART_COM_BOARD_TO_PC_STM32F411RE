# Projet de Communication USART pour STM32F411RE

Ce projet est un modèle de base pour la communication USART (port série) entre une carte de développement STM32F411RE (telle que la NUCLEO-F411RE) et un ordinateur.

Il est configuré à l'aide de STM32CubeIDE et utilise la couche d'abstraction matérielle (HAL) de ST.

## Matériel Requis

*   Une carte de développement basée sur le STM32F411RE (ex: **NUCLEO-F411RE**).
*   Un câble USB pour connecter la carte à l'ordinateur (généralement Type-A vers Mini-B ou Micro-B).

## Logiciels Requis

*   [**STM32CubeIDE**](https://www.st.com/en/development-tools/stm32cubeide.html)
*   Un logiciel de terminal série sur votre PC, comme :
    *   [PuTTY](https://www.putty.org/) (Windows)
    *   [Tera Term](https://ttssh2.osdn.jp/index.html.en) (Windows)
    *   `minicom` ou `screen` (Linux/macOS)

## Configuration du Projet

Le projet est pré-configuré avec les paramètres suivants :

*   **Microcontrôleur** : STM32F411RE
*   **Périphérique de communication** : `USART2`. Sur les cartes Nucleo, l'USART2 est directement connecté au port COM virtuel du ST-Link, ce qui simplifie la connexion au PC.
*   **Paramètres de la liaison série** :
    *   **Baud Rate** : 115200 bps
    *   **Data Bits** : 8
    *   **Parity** : None
    *   **Stop Bits** : 1

## Mise en Route

1.  **Clonez** ce dépôt sur votre machine locale.
2.  **Ouvrez** le projet dans STM32CubeIDE (`File > Open Projects from File System...`).
3.  **Compilez** le projet en cliquant sur l'icône du marteau (Build).
4.  **Téléversez** le programme sur la carte en cliquant sur l'icône de lecture verte (Run).

## Comment Utiliser

Par défaut, la boucle principale dans `Core/Src/main.c` est vide. Le matériel est initialisé, mais aucune donnée n'est envoyée.

Pour envoyer un message "Hello, World!" toutes les secondes, suivez ces étapes :

1.  Ouvrez le fichier `USART_COM_BOARD_TO_PC/Core/Src/main.c`.
2.  Ajoutez le code suivant pour envoyer une chaîne de caractères via l'UART.

    ```c
    /* Includes ------------------------------------------------------------------*/
    #include "main.h"
    #include <string.h> // <-- Ajoutez cette ligne

    // ...

    /* Private variables ---------------------------------------------------------*/
    TIM_HandleTypeDef htim2;

    UART_HandleTypeDef huart2;

    /* USER CODE BEGIN PV */
    char msg[] = "Hello, World!\r\n"; // <-- Message à envoyer
    /* USER CODE END PV */

    // ...

    int main(void)
    {
      // ... (code d'initialisation existant)

      /* Infinite loop */
      /* USER CODE BEGIN WHILE */
      while (1)
      {
        /* USER CODE END WHILE */

        /* USER CODE BEGIN 3 */
        // On envoie le message via l'USART2
        HAL_UART_Transmit(&huart2, (uint8_t*)msg, strlen(msg), HAL_MAX_DELAY);
        
        // On attend 1 seconde
        HAL_Delay(1000);
      }
      /* USER CODE END 3 */
    }
    ```

3.  Recompilez et téléversez à nouveau le projet sur la carte.
4.  Ouvrez votre logiciel de terminal série sur votre PC.
5.  Connectez-vous au port COM correspondant à votre carte STM32 avec les paramètres **115200 8N1**.
6.  Vous devriez voir le message "Hello, World!" s'afficher toutes les secondes.

## Structure du Projet

*   `Core/` : Contient les fichiers principaux de l'application (`main.c`, `stm32f4xx_it.c`, etc.).
*   `Drivers/` : Contient les drivers HAL et les fichiers de démarrage du microcontrôleur.
*   `USART_COM_BOARD_TO_PC.ioc` : Le fichier de configuration STM32CubeMX. Vous pouvez l'ouvrir pour modifier la configuration matérielle du projet.
