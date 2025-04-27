# Observer-Design-Pattern



Observer Design Pattern :
-----------------------

The Observer Pattern is a behavioral design pattern where:

		- An object (Subject) maintains a list of its dependents (Observers).
		- It notifies all observers automatically when its state changes.
		- Observer Pattern is perfect when you have one-to-many relationships.
        - When the Subject changes, all Observers are notified automatically.
        - It promotes low coupling between Subject and Observers.



Simple Meaning:

		- When the subject changes, all observers are updated automatically.
		
Real-world Example:

		Think about YouTube:

			You subscribe to a channel (you are an Observer).
			When the channel uploads a new video (Subject changes state),
			You get notified about it.
			✅ You don't need to check the channel manually every time.


Components of Observer Pattern:
-------------------------------

			Part													Description
			
			Subject													Holds the data, list of observers, and notifies them on changes
			Observer												Gets updated when the subject's state changes
			Concrete Subject										Real subject class
			Concrete Observer										Real observer class
			
Java Code Example :
-----------------

Let's build a Weather Station example 🌦️:

		* WeatherData (Subject) changes temperature.
		* MobileDisplay and LaptopDisplay (Observers) get updates.
		

Step 1: Create the Observer Interface

			// Observer.java
			public interface Observer {
				void update(float temperature);
			}

Step 2: Create the Subject Interface


			// Subject.java
			public interface Subject {
				void registerObserver(Observer o);
				void removeObserver(Observer o);
				void notifyObservers();
			}
			
Step 3: Concrete Subject (WeatherData)		

			// WeatherData.java
			import java.util.ArrayList;
			import java.util.List;

			public class WeatherData implements Subject {
				private List<Observer> observers;
				private float temperature;

				public WeatherData() {
					observers = new ArrayList<>();
				}

				@Override
				public void registerObserver(Observer o) {
					observers.add(o);
				}

				@Override
				public void removeObserver(Observer o) {
					observers.remove(o);
				}

				@Override
				public void notifyObservers() {
					for (Observer observer : observers) {
						observer.update(temperature);
					}
				}

				public void setTemperature(float temperature) {
					this.temperature = temperature;
					notifyObservers();
				}
			}
			

Step 4: Concrete Observers (MobileDisplay & LaptopDisplay)


				// MobileDisplay.java
				public class MobileDisplay implements Observer {
					private String displayId;

					public MobileDisplay(String displayId) {
						this.displayId = displayId;
					}

					@Override
					public void update(float temperature) {
						System.out.println("Mobile Display " + displayId + ": Temperature updated to " + temperature + "°C");
					}
				}

				// LaptopDisplay.java
				public class LaptopDisplay implements Observer {
					private String displayId;

					public LaptopDisplay(String displayId) {
						this.displayId = displayId;
					}

					@Override
					public void update(float temperature) {
						System.out.println("Laptop Display " + displayId + ": Temperature updated to " + temperature + "°C");
					}
				}

Step 5: Test the Observer Pattern

				// Main.java
				public class Main {
					public static void main(String[] args) {
						WeatherData weatherData = new WeatherData();

						Observer mobileDisplay = new MobileDisplay("M1");
						Observer laptopDisplay = new LaptopDisplay("L1");

						weatherData.registerObserver(mobileDisplay);
						weatherData.registerObserver(laptopDisplay);

						// Weather changes
						weatherData.setTemperature(25.0f);
						weatherData.setTemperature(30.5f);

						// Unregister laptop display
						weatherData.removeObserver(laptopDisplay);

						// Weather changes again
						weatherData.setTemperature(28.2f);
					}
				}

output:

			Mobile Display M1: Temperature updated to 25.0°C
			Laptop Display L1: Temperature updated to 25.0°C
			Mobile Display M1: Temperature updated to 30.5°C
			Laptop Display L1: Temperature updated to 30.5°C
			Mobile Display M1: Temperature updated to 28.2°C


Why Use Observer Pattern?

			Advantage											Reason
			Loose Coupling										Subject and Observer are independent
			Dynamic Updates										Observers can register/unregister at any time
			Scalable											Easily add more observers without changing Subject
			Reusability											Observers and Subjects can be reused separately
