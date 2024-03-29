# Router
Extension to use namespace to better organize routes
Works with any framework that uses the @app.route("/") style for route declaration 

# Usage
Router to handle routes
Adds the set namespace to all route set by the class instance

Parameters:
  During Creation:
    base url: '/auth'
    app: wsgi app
    routes: a list of your app routes
  Calling route method:
    the default routes arguments

``` python
    # Router.route:
    auth = Router('/auth', app)
    @auth.route(url='/login', method="GET")
    def login():
        ...

    # produces:
        app.route('/auth/login', method="GET")

    # Router.new and Router.router:
    auth = Router('/auth', app)

    def login():
       ...

    # Create route
    auth.new(url='/login', func=login, method='POST')

    # Instantiate all created routes
    auth.router()

    # You can also call Router.router() directly and pass a url routes argument
    app = Bottle() or Flask(__file__)
    api = Router('/', app)

    def getProducts():
        response['content-type'] = 'application/json'
        data = db.get()
        return data

    def getProductsId(id):
        try:
            response['content-type'] = 'application/json'
            response['status-code'] = 200
            data = db.get(id=id)
            return data
        except:
            response['status-code'] = 400

    def addProduct():
        data = {
            'icon': request.POST,
            'name': request.POST.name,
            'category': request.POST.category,
            'date': datetime,
            'experience': request.POST.experience,
            'work_level': request.POST.work_level,
            'employee_type': request.POST.employee_level,
            'offer_salary': request.POST.offer_salary,
            'overview': request.POST.overview,
            'description': request.POST.description
        }
        try:
            db.add(data)
            response['status-code'] = 200
        except:
            response['status-code'] = 400
    routes = {}
    routes["/api"] = {
        "/products": (getProducts, "GET", "get-products"), 
        "/new" : {
            "/products" : (addProduct, "POST", "add")
           }
    }
    routes["/api"]["/products/<id:int>"] = (getProductsId, "GET", "single")
    api.router(routes)

    # Or use router_v2

    api.router_v2([
            '/api', # Base url for all routes defined here
            {
                'url': '/products',
                'method': 'GET',
                'func': getProducts,
            }, # A route instance.. produces: app.route('/api/products', method='GET', callback=getProducts)
            [
                '/new',
                {
                   'url': '/products',
                   'method': 'POST',
                   'func': addProduct
                }
            ],# A subroute instance.. produces: app.route('/api/new/products', method='POST', callback=addProduct)
            {
                'url': '/products/<id:int>',
                'method': 'GET',
                'func': getProductsId,
            },
        ])
```
