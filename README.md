# passport-LogRegister

                                 How to install Passport in Laravel

Step 1. composer require laravel/passport

Step 2.php artisan migrate


Step 3.php artisan passport:install

Step 4.open App\Models\User and put the below code in that
use Laravel\Passport\HasApiTokens; 
,HasApiTokens


Step 5.open App\Providers\AuthServiceProvider

use Laravel\Passport\Passport;
public function boot()
    {
        $this->registerPolicies();

        Passport::routes();
    }
	
Step 6.open config/auth.php and put the below code

'api' => [
        'driver' => 'passport', //update this line
        'provider' => 'users',
    ],


Step 7.open App\Models\User and put the below code in that
use Laravel\Passport\HasApiTokens;
, HasApiTokens	



////////////////////////////////////////////////////////////////////////////////////////////////////////////

                       Create API Rest with Laravel 8 Passport Authentication 
					   
					   

step 1: Creating our controllers 
php artisan make:controller Auth/UserAuthController 

put the below code in that

use App\Models\User;
use Validator;
use Illuminate\Support\Facades\Auth;


    public function register(Request $request) {
        $request->validate([
            'name' => 'required|string',
            'email' => 'required|string|email|unique:users',
            'password' => 'required|string|confirmed'
        ]);
        $user = new User([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password)
        ]);
        $user->save();
        return response()->json([
            'message' => 'Successfully created user!'
        ], 201);
    }




    
    public function login(Request $request){
    	$validator = Validator::make($request->all(), [
            'email' => 'required|email',
            'password' => 'required|string|min:6',
        ]);

        if ($validator->fails()) {
            return response()->json($validator->errors(), 422);
        }

        if (! $token = auth()->attempt($validator->validated())) {
            return response()->json(['error' => 'Either email or password is wrong.'], 401);
        }
        return response()->json([
            'message' => 'Successfully created user!'
        ], 201);
    }

	
	
step 2: put the below code in routes	
Route::group([
    'prefix' => 'auth'
], function () {
    Route::post('login', 'App\Http\Controllers\Auth\UserAuthController@login');
    Route::post('register', 'App\Http\Controllers\Auth\UserAuthController@register');
  
});

step 3:
now put this address in postman
http://localhost:8000/api/auth/register	



i got forget to explain in model of user in laravel and lets go to say a point
