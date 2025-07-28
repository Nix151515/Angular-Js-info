    Template-driven vs Reactive




    ControlValueAccessor
To synchronize a custom form component with Angular's form control (used with [(ngModel)] or formControlName).
To implement how the value is written to the view and how changes are communicated back to the form model.

ControlValueAccessor lets Angular forms interact with custom components like they do with native form elements. 
This is especially useful when creating reusable or third-party UI components (e.g., dropdowns, sliders, datepickers).

interface ControlValueAccessor {
  writeValue(obj: any): void;
  registerOnChange(fn: any): void;
  registerOnTouched(fn: any): void;
  setDisabledState?(isDisabled: boolean): void;
}

-writeValue(obj: any): Writes a new value from the form model into the view.
-registerOnChange(fn: any): Registers a callback that should be called when the value changes in the UI.
-registerOnTouched(fn: any): Registers a callback for when the control is touched (blurred).
-setDisabledState?(isDisabled: boolean): (Optional) Handles the disabled state of the control.

The NG_VALUE_ACCESSOR token is needed to tell Angular how to access and write values to a form control.



      NG_VALUE_ACCESSOR

NG_VALUE_ACCESSOR is a multi-provider token used by Angular forms to register custom components as value accessors. When Angular processes a form control, it checks for this token to determine how to read from and write to that control.

It’s defined like this internally:
export const NG_VALUE_ACCESSOR: InjectionToken<ControlValueAccessor[]> = new InjectionToken('NgValueAccessor');

When you create a custom form control component (like a custom input, toggle switch, etc.), Angular doesn't automatically know how to interact with it.
By providing NG_VALUE_ACCESSOR, you tell Angular:
"This component knows how to behave like a form control — it can read, write, notify changes, and handle touches."

You register it in the @Component decorator using the providers array:
providers: [
  {
    provide: NG_VALUE_ACCESSOR,                            // register as value accessor
    useExisting: forwardRef(() => CustomInputComponent),   // points to the current component
    multi: true
  }
]
