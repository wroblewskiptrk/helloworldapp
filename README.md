# Lab - tworzenie zasobów

## Wymagania

Aktywna subskrypcja w Azure i dostęp do portalu.

## Wstęp

### Cel

Stworzenie Web App oraz zintegrowanie z GitHub.

Czas trwania: 30 minut

## Typy zasobów

`Resource group` - pudełko na zasoby w chmurze
`Web App` - serwer aplikacji


### Krok 0 - Uruchom Cloud Shell w Azure i sklonuj kod ćwiczeń

Nawiguj w przeglądarce do [portal.azure.com](https://portal.azure.com), uruchom "Cloud Shell" i wybierz `Bash`.

Oficjalna dokumentacja: [Cloud Shell Quickstart](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/cloud-shell/quickstart.md).

```bash
git clone https://github.com/wguzik/helloworldapp.git
```

> Poniższe kroki realizuje się za pomocą Cloud Shell.

### Krok 1 - stwórz zasoby


  ```bash
    # Skopiuj poniższy blok do Cloud Shell i zastąp zmienne odpowiednimi wartościami, potwierdź enterem
    RESOURCE_GROUP="merito-rg"
    LOCATION="eastus"
    APP_NAME="mywebapp$RANDOM"
    APP_NAME_SETTING="MyPowerfulApp"
    APP_SERVICE_PLAN="myAppServicePlan"
    SKU="B1"
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

az webapp up \
    --name $APP_NAME \
    --resource-group $RESOURCE_GROUP \
    --runtime $RUNTIME

  ```

- pobierz adres URL Web App
  ```bash
  WEBAPP_URL=$(az webapp show --name $APP_NAME --resource-group $RESOURCE_GROUP --query "defaultHostName" -o tsv)
echo "Your web app is deployed at: https://$WEBAPP_URL"
  ```

### Krok 8 - Usuń zasoby

```
az group delete --name $RESOURCE_GROUP --yes --no-wait
```

## Zadanie domowe
Dodaj więcej do zmiennej `environment` ograniczenie dotyczące możliwych wartości, np 'dev', 'test', 'prod'.