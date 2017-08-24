---
layout: post
title:   KVO & Registering Dependent Keys
author: Devin McKaskle
categories: [mapkit, kvo]
---

Registering dependent keys isn't something you have to do very often, but it can be convenient in certain situations. I'll illustrate the concept with a MapKit example.

Before I get into dependent keys though, let's look at how MapKit uses [Key-Value Observing][KVO] to keep map pins up-to-date with their underlying annotations. Here's a simple implementation of a map annotation. You will notice that I added `dynamic` keyword to the annotation properties in order to opt into KVO when working in Swift.

[KVO]: https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html

{% highlight swift %}
final class Annotation: NSObject, MKAnnotation {
   dynamic var title: String? = "before"
   dynamic var coordinate = CLLocationCoordinate2D(latitude: 38, longitude: -100)
}
{% endhighlight %}

To demonstrate how this works, I have a view controller with a map. We add the annotation to the map and change the title of the annotation when a button is pressed.

{% highlight swift %}
class ViewController: UIViewController {
   private let annotation = Annotation()
   @IBOutlet private weak var map: MKMapView!
   
   override func viewDidLoad() {
      super.viewDidLoad()
      map.addAnnotation(annotation)
   }
   
   @IBAction private func buttonTapped() {
      annotation.title = "after"
   }
}
{% endhighlight %}

Here is the result:

<div class="row">
   <div class="large-6 columns">
      <img src="{{ site.url }}/img/posts/kvo-and-registering-dependent-keys/before.jpg">
      <h5 class="text-center">Initial state</h5>
   </div>
   <div class="large-6 columns">
      <img src="{{ site.url }}/img/posts/kvo-and-registering-dependent-keys/after.jpg">
      <h5 class="text-center">After button press</h5>
   </div>
</div>

As you can see, all we had to do was change the title of the annotation and the map automatically updated the pin's title to reflect the new value. We did not have to interact with the map at all. This is the beauty of KVO.


Let's move to a more real-world example where the model object doesn't already conform to the `MKAnnotation` protocol.

{% highlight swift %}
final class Delivery {
   enum Status { 
      case inTransit, outForDelivery, delivered 
      
      var localizedValue: String {
         switch self {
         case .inTransit: return NSLocalizedString("In Transit", comment: "delivery is being transferred within the shipping network")
         case .outForDelivery: return NSLocalizedString("Out for Delivery", comment: "delivery is on the last truck, about to be delivered")
         case .delivered: return NSLocalizedString("Delivered", comment: "delivery has reached its destination")
         }
      }
   }
   
   var packageName: String
   var status: Status
   var latitude: Double
   var longitude: Double
}
{% endhighlight %}


It is straightforward to add `MKAnnotation` conformance to `Delivery`:

{% highlight swift %}
extension Delivery: MKAnnotation {
   var title: String? { return packageName }
   var subtitle: String? { return status.localizedValue }
   var coordinate: CLLocationCoordinate2D { return CLLocationCoordinate2D(latitude: latitude, longitude: longitude) }
}
{% endhighlight %}


However, if we stop there, the map can't automatically update a pin for this annotation because it's only observing `title`, `subtitle`, and `coordinate`. Those properties are read-only, so you won't ever be changing them directly. Instead, you will be modifying `status`, `latitude`, etc. like this:

{% highlight swift %}
let delivery: Delivery = … // the delivery that is on the map
delivery.status = .delivered
delivery.latitude = 36
delivery.longitude = -92
{% endhighlight %}

We need a way to tell the system, "If you are interested in knowing when `subtitle` changes, then you should really be observing `status` instead". This is where [registering dependent keys][registering-dependent-keys] comes in.

[registering-dependent-keys]: https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html


To register dependent keys, you need to implement:

{% highlight swift %}
class func keyPathsForValuesAffecting<#Key#>() -> Set<String>
{% endhighlight %}

`<#Key#>` is each dependent property you want to register. Below, you can see how to do this for `Delivery`.


{% highlight swift %}
extension Delivery: MKAnnotation {
   var title: String? { return packageName }
   var subtitle: String? { return status.localizedValue }
   var coordinate: CLLocationCoordinate2D { return CLLocationCoordinate2D(latitude: latitude, longitude: longitude) }
   
   class func keyPathsForValuesAffectingTitle() -> Set<String> {
      return [#keyPath(Delivery.name)]
   }

   class func keyPathsForValuesAffectingSubtitle() -> Set<String> {
      return [#keyPath(Delivery.distance)]
   }

   class func keyPathsForValuesAffectingCoordinate() -> Set<String> {
      return [#keyPath(Delivery.latitude), #keyPath(Delivery.longitude)]
   }
}
{% endhighlight %}

With these methods in place, the system now knows to observe `name` instead of `title`, `distance` instead of `subtitle`, and `latitude` and `longitude` instead of `coordinate`.

Note that `name`, `distance`, `latitude` and `longitude` need to be marked as `dynamic` too now that the system is going to observe them. If you don't mark them `dynamic`, they will not inform any observers when changes occur.

{% highlight swift %}
final class Delivery {
   enum Status { … }
   
   dynamic var packageName: String
   dynamic var status: Status
   dynamic var latitude: Double
   dynamic var longitude: Double
}
{% endhighlight %}

And now, just as with the first annotation we looked at, the map view will automatically update its pin when a `Delivery` instance is modified. You can find out more information about why this works in the [documentation for `keyPathsForValuesAffectingValue(forKey:)`][keypaths].

[keypaths]: https://developer.apple.com/reference/objectivec/nsobject/1414299-keypathsforvaluesaffectingvalue