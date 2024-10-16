# Lab - Apka w wielu regionach i integracja z GitHub

## Wymagania

Aktywna subskrypcja w Azure i dostęp do portalu.

## Wstęp

### Cel

Stworzenie Web App oraz zintegrowanie z GitHub.

Czas trwania: 30 minut

## Typy zasobów

`Resource group` - pudełko na zasoby w chmurze
`Web App` - serwer aplikacji


### Krok 0 - Zrób fork repozytorium

- Przejdź do [repozytorium](https://github.com/wguzik/helloworldapp)

- Kliknij `Fork` (w prawym górnym rogu)

- Wybierz swoje konto

- Poczekaj, aż fork zostanie utworzony

### Krok 1 - Sklonuj repozytorium w Cloud Shell

Nawiguj w przeglądarce do [portal.azure.com](https://portal.azure.com), uruchom "Cloud Shell" i wybierz `Bash`.

> Oficjalna dokumentacja: [Cloud Shell Quickstart](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/cloud-shell/quickstart.md).

```bash
git clone https://github.com/<TwojeRepo>/helloworldapp.git

cd helloworldapp
```

> Poniższe kroki realizuje się za pomocą Cloud Shell.

### Krok 2 - stwórz zasoby

  ```bash
    # Skopiuj poniższy blok do Cloud Shell i zastąp zmienne odpowiednimi wartościami, potwierdź enterem
    RESOURCE_GROUP="merito-rg"
    LOCATION="northeurope"
    APP_NAME="mywebapp$RANDOM"
    APP_NAME_SETTING="MyPowerfulApp"
    APP_SERVICE_PLAN="myAppServicePlan"
    SKU="P0v3"
    RUNTIME="NODE:20-lts"

    # Stwórz resource group
    az group create --name $RESOURCE_GROUP --location $LOCATION
    
    # Stwórz plan usługi App Service
    az appservice plan create \
    --name $APP_SERVICE_PLAN \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --sku $SKU \
    --is-linux

    # Stwórz Web App
    az webapp create \
    --name $APP_NAME \
    --resource-group $RESOURCE_GROUP \
    --plan $APP_SERVICE_PLAN \
    --runtime $RUNTIME

    az webapp config appsettings set \
    --name $APP_NAME \
    --resource-group $RESOURCE_GROUP \
    --settings APP_NAME=$APP_NAME_SETTING
  ```

- pobierz adres URL Web App

  ```bash
  WEBAPP_URL=$(az webapp show --name $APP_NAME --resource-group $RESOURCE_GROUP --query "defaultHostName" -o tsv)

  echo "Twoja aplikacja działa pod adresem: https://$WEBAPP_URL"
  ```

### Krok 3 - Zintegruj z GitHub

- Z poziomu portalu Azure przejdź do swojego Web App
- W sekcji `Deployment Center` wybierz `GitHub`
- Postępuj zgodnie z instrukcjami, aby autoryzować dostęp do GitHuba i wybrać repozytorium
- wybierz repozytorium `helloworldapp`
- wybierz branch `main`
- wybierz "Basic authentication"
- wybierz "Preview file"
- wybierz "Save"

### Krok 4 - Odwiedź swoją aplikację

- pobierz adres URL Web App
  ```bash
  WEBAPP_URL=$(az webapp show --name $APP_NAME --resource-group $RESOURCE_GROUP --query "defaultHostName" -o tsv)
  
  echo "Twoja aplikacja działa pod adresem: https://$WEBAPP_URL"
  ```

### Krok 5 - Zaktualizuj kod

- otwórz GitHub w przeglądarce, otwórz repozytorium `helloworldapp`
- edytuj plik `src/index.js` przez zmianę tekstu w linii 29
- wypchnij zmiany do GitHuba
- zobacz, że zmiany są widoczne na stronie

### Krok 6 - Zrób pull request

- przejdź do swojego repozytorium na GitHubie
- otwórz branch o nazwie `update hello message`
- zacommituj zmianę
- stwórz pull request przez wciśnięcie "Compare & pull request"
- wklej poniższy tekst do opisu

> Tekst powitania odpowiada wersji językowej 

- wciśnij "Create pull request"
- wciśnij "Merge pull request"
- wciśnij "Delete branch"

### Krok 7 - Sprawdź, czy zmiany są widoczne

- odwiedź swoją aplikację
- zobacz, że zmiany są widoczne

### Krok 8 - GitHub Actions

- otwórz GitHub w przeglądarce, przejdź do swojego repozytorium
- w sekcji `Actions` obejrzyj, jak działa GitHub Actions

### Krok -1 - Usuń zasoby

```
az group delete --name $RESOURCE_GROUP --yes --no-wait
```
