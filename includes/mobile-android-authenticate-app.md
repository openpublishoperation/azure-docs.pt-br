
1. No **Gerenciador de projetos** , no Android Studio, abra o arquivo ToDoActivity.java e adicione as seguintes instruções de importação.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
2. Adicione o método a seguir à classe **ToDoActivity** :

        // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;
 
        private void authenticate() {
            // Login using the Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
         
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check the request code matches the one we send in the login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check the error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    Isso cria um novo método para manipular o processo de autenticação. O usuário é autenticado usando uma credencial do Google. Um diálogo exibe a ID do usuário autenticado. Você não pode continuar sem uma autenticação positiva.

    > [!NOTE]
    > Se você estiver usando um provedor de identidade diferente do Google, altere o valor passado ao método **login** acima para um destes: _MicrosoftAccount_, _Facebook_, _Twitter_ ou _windowsazureactivedirectory_.

3. No método **OnCreate**, adicione a linha de código a seguir após o código que cria uma instância do objeto `MobileServiceClient`.

        authenticate();

    Essa chamada inicia o processo de autenticação.
4. Mova o código restante após `authenticate();` no método **OnCreate** para um novo método **CreateTable**. Isso se parece com o seguinte:

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }

5. Adicione o seguinte trecho de _RedirectUrlActivity_ a _AndroidManifest.xml_ para garantir que o redirecionamento funcione.
 
        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

6.  Adicione redirectUriScheme a _build.gradle_ de seu aplicativo Android.
 
        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

7. Adicione com.android.support:customtabs:23.0.1 às dependências em seu build.gradle:

      dependências {        // ...        compile 'com.android.support:customtabs:23.0.1'    }

8. No menu **Executar**, clique em **Executar aplicativo** para iniciar o aplicativo e entrar com seu provedor de identidade escolhido.

Ao entrar com êxito, o aplicativo deve ser executado sem erros e você deve ser capaz de consultar o serviço de back-end e fazer atualizações nos dados.
