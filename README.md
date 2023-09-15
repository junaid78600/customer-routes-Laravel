# Create Customes file for routes

## Getting Started
### Step 1: Create files that you want like manager.php in routes

I created three file which are these,
1) user.php
2) manager.php
3) admin.php
   
   ![files](https://github.com/junaid78600/customer-routes-Laravel/assets/45033213/1bb1c212-f449-41d6-84c5-18cc5d728e42)

### And write this code in these files

````

    <?php
    
    Route::get('/', function () {
        dd('Welcome to manager routes.');
    });
````

### Step 2: Go to app/providers/RouteServiceProvider.php
write this code
   
````
        <?php
    
                namespace App\Providers;
                
                use Illuminate\Cache\RateLimiting\Limit;
                use Illuminate\Foundation\Support\Providers\RouteServiceProvider as ServiceProvider;
                use Illuminate\Http\Request;
                use Illuminate\Support\Facades\RateLimiter;
                use Illuminate\Support\Facades\Route;
                
                class RouteServiceProvider extends ServiceProvider
                {
                    /**
                     * The path to your application's "home" route.
                     *
                     * Typically, users are redirected here after authentication.
                     *
                     * @var string
                     */
                    public const HOME = '/home';
                
                    /**
                     * Define your route model bindings, pattern filters, and other route configuration.
                     */
                    public function boot(): void
                    {
                        RateLimiter::for('api', function (Request $request) {
                            return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());
                        });
                
                        $this->routes(function () {
                            Route::middleware('api')
                                ->prefix('api')
                                ->group(base_path('routes/api.php'));
                
                            Route::middleware('web')
                                ->group(base_path('routes/web.php'));
                
                            Route::prefix('admin')
                                ->namespace($this->namespace)
                                ->group(base_path('routes/admin.php'));
                
                            Route::prefix('manager')
                                ->namespace($this->namespace)
                                ->group(base_path('routes/manager.php'));
                
                            Route::prefix('user')
                                ->namespace($this->namespace)
                                ->group(base_path('routes/user.php'));
                        });
                    }
                
                    public function map()
                    {
                
                        $this->mapApiRoutes();
                
                        $this->mapWebRoutes();
                
                        $this->mapAdminRoutes();
                
                
                        $this->mapManagerRoutes();
                
                        $this->mapUserRoutes();
                    }
                
                }

````
### Step 2: Visit the link like

````
    1) http://localhost:8000/users/*

    2) http://localhost:8000/manager/*
    
    3) http://localhost:8000/admin/*
````


    
