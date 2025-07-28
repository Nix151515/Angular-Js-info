null
  ↑
PlatformInjector
  ↑
RootInjector (AppModule or Standalone)
  ↑
ModuleInjector (optional, lazy-loaded/feature)
  ↑
ComponentInjector (@Component.providers)
  ↑
ElementInjector (per DOM node)

Angular facilitate the interaction from dependencies consumer to dependency providers through Injector.

1. Platform injector
platformBrowserDynamic().bootstrapModule(AppModule)
Rarely used directly, this is the injector root, created on app start

2. Root Injector (recommended)
 @Injectable({ providedIn: 'root' }) services
 providers:[] from AppModule
 Singleton instances, living there the entire lifetime of the app
 It is treeshaked for the components not needing it.

3. Module Injector:
providers:[] in a module
Useful for lazy-loaded modules, feature isolation or scoped services

4. Component Injector:
@Component({
    ...
  providers: [HeroService]
})
class HeroListComponent {}

You get a new instance of the service with each new instance of that component.
It is available to all the children from the DOM (pipes, directives).
Useful for stateful services (dialogs, forms)

5. Element Injector (per DOM element)
Each Angular element (component or directive) has its own injector.
These injectors form a tree that mirrors the component DOM hierarchy.
Angular climbs up this tree when resolving dependencies at runtime.
If Angular doesn't find the provider in any ElementInjector hierarchies, it goes back to the element where the request originated and looks in the EnvironmentInjector hierarchy.
If Angular still doesn't find the provider, it throws an error.
An ElementInjector is empty by default unless you configure it in the providers property on @Directive() or @Component().



    Ways to provide dependecies

    The provide property holds the token that serves as the key for both locating a dependency value and configuring the injector.
The second property is a provider definition object, which tells the injector how to create the dependency value.
The provider-definition key can be one of the following:

- useClass - this option tells Angular DI to instantiate a provided class when a dependency is injected
[{ provide: Logger, useClass: Logger }]
  Shorthand : 
providers: [Logger] 
  Another class can be used for useClass (extended classes, mock classes for testing), but provide also their other dependencies if any.

- useExisting - allows you to alias a token and reference any existing one.
[ NewLogger,
  // Alias OldLogger w/ reference to NewLogger
  { provide: OldLogger, useExisting: NewLogger}]

- useFactory - allows you to define a function that constructs a dependency.
Ex: Create a factory based on a variable from another service and provide that.

constructor(
  private logger: Logger,
  private isAuthorized: boolean) { }

getHeroes() {
  const auth = this.isAuthorized ? 'authorized ' : 'unauthorized';
  this.logger.log(`Getting heroes for ${auth} user.`);
  return HEROES.filter(hero => this.isAuthorized || !hero.isSecret);
}
..........
const heroServiceFactory = (logger: Logger, userService: UserService) =>
  new HeroService(logger, userService.user.isAuthorized);
............
export const heroServiceProvider =
  { provide: HeroService,
    useFactory: heroServiceFactory,
    deps: [Logger, UserService]
  };

- useValue - provides a static value that should be used as a dependency.
[{ provide: TestData, useValue: 'test' }]




    Injection Tokens
  Can be used for non-class dependencies

export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');
---
providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]
---
constructor(@Inject(APP_CONFIG) config: AppConfig) {
  this.title = config.title;
}

The optional type parameter, <AppConfig>, and the token description, app.config, specify the token's purpose.



   DI Decorators:
- @Self()
constructor(@Self() myService: MyService) {}
If the service isn't found in the current injector, Angular throws an error.
Use case: You want to enforce that the dependency must be provided locally, like in the component’s providers.

- @SkipSelf()
constructor(@SkipSelf() myService: MyService) {}
Skip the current injector and look in ancestors only (from the parent above).
Useful when a service might be shadowed locally, but you want the parent version.
Use case: You override a service locally, but a child component still wants the parent’s instance (e.g., nested forms or hierarchical logging).

- @Host()
constructor(@Host() myService: MyService) {}
Look only at the host (parent) element, not the entire hierarchy.

// host.component.ts
@Component({
  selector: 'app-host',
  template: `<app-child></app-child>`,
  providers: [LoggerService]
})
export class HostComponent { }


// child.component.ts
@Component({
  selector: 'app-child',
  template: `<p>Child works!</p>`
})
export class ChildComponent {
  constructor(@Host() private logger: LoggerService) {
    this.logger?.log('ChildComponent initialized');
  }
}


- @Optional()
constructor(@Optional() myService: MyService | null) {}
Makes the dependency optional.
Instead of throwing an error, it injects null.







 Docs:

https://angular.dev/guide/di/hierarchical-dependency-injection
https://v17.angular.io/guide/dependency-injection
https://github.com/cochiletSerban/interview-god/blob/master/angular/dependency-injection/dependency-injection.md
