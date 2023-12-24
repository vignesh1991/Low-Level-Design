classDiagram
direction BT
class AlreadyExistsException {
  + AlreadyExistsException() 
}
class BadRequestException {
  + BadRequestException() 
}
class Booking {
  + Booking(String, Show, String, List~Seat~) 
  - Show show
  - String id
  - List~Seat~ seatsBooked
  - BookingStatus bookingStatus
  - String user
  + confirmBooking() void
  + expireBooking() void
   BookingStatus bookingStatus
   boolean confirmed
   Show show
   List~Seat~ seatsBooked
   String id
   String user
}
class BookingController {
  + BookingController(ShowService, BookingService, TheatreService) 
  + createBooking(String, String, List~String~) String
}
class BookingService {
  + BookingService(SeatLockProvider) 
  + confirmBooking(Booking, String) void
  + getAllBookings(Show) List~Booking~
  + getBookedSeats(Show) List~Seat~
  + getBooking(String) Booking
  + createBooking(String, Show, List~Seat~) Booking
  - isAnySeatAlreadyBooked(Show, List~Seat~) boolean
}
class BookingStatus {
<<enumeration>>
  + BookingStatus() 
  + values() BookingStatus[]
  + valueOf(String) BookingStatus
}
class InMemorySeatLockProvider {
  + InMemorySeatLockProvider(Integer) 
  + lockSeats(Show, List~Seat~, String) void
  + getLockedSeats(Show) List~Seat~
  + unlockSeats(Show, List~Seat~, String) void
  + validateLock(Show, Seat, String) boolean
  - unlockSeat(Show, Seat) void
  - isSeatLocked(Show, Seat) boolean
  - lockSeat(Show, Seat, String, Integer) void
}
class InvalidStateException {
  + InvalidStateException() 
}
class Movie {
  + Movie(String, String) 
  - String id
  - String name
   String name
   String id
}
class MovieController {
  + MovieController(MovieService) 
  + createMovie(String) String
}
class MovieService {
  + MovieService() 
  + getMovie(String) Movie
  + createMovie(String) Movie
}
class NotFoundException {
  + NotFoundException() 
}
class PaymentsController {
  + PaymentsController(PaymentsService, BookingService) 
  + paymentFailed(String, String) void
  + paymentSuccess(String, String) void
}
class PaymentsService {
  + PaymentsService(Integer, SeatLockProvider) 
  + processPaymentFailed(Booking, String) void
}
class Screen {
  + Screen(String, String, Theatre) 
  - String name
  - Theatre theatre
  - String id
  - List~Seat~ seats
  + addSeat(Seat) void
   String name
   String id
   List~Seat~ seats
   Theatre theatre
}
class ScreenAlreadyOccupiedException {
  + ScreenAlreadyOccupiedException() 
}
class Seat {
  + Seat(String, int, int) 
  - int seatNo
  - String id
  - int rowNo
   String id
   int rowNo
   int seatNo
}
class SeatAvailabilityService {
  + SeatAvailabilityService(BookingService, SeatLockProvider) 
  + getAvailableSeats(Show) List~Seat~
  - getUnavailableSeats(Show) List~Seat~
}
class SeatLock {
  + SeatLock(Seat, Show, Integer, Date, String) 
  - Seat seat
  - Integer timeoutInSeconds
  - String lockedBy
  - Date lockTime
  - Show show
   String lockedBy
   Seat seat
   Date lockTime
   Show show
   Integer timeoutInSeconds
   boolean lockExpired
}
class SeatLockProvider {
<<Interface>>
  + validateLock(Show, Seat, String) boolean
  + unlockSeats(Show, List~Seat~, String) void
  + lockSeats(Show, List~Seat~, String) void
  + getLockedSeats(Show) List~Seat~
}
class SeatPermanentlyUnavailableException {
  + SeatPermanentlyUnavailableException() 
}
class SeatTemporaryUnavailableException {
  + SeatTemporaryUnavailableException() 
}
class Show {
  + Show(String, Movie, Screen, Date, Integer) 
  - Movie movie
  - Date startTime
  - String id
  - Screen screen
  - Integer durationInSeconds
   Date startTime
   Movie movie
   Integer durationInSeconds
   Screen screen
   String id
}
class ShowController {
  + ShowController(SeatAvailabilityService, ShowService, TheatreService, MovieService) 
  + getAvailableSeats(String) List~String~
  + createShow(String, String, Date, Integer) String
}
class ShowService {
  + ShowService() 
  + getShow(String) Show
  + createShow(Movie, Screen, Date, Integer) Show
  - checkIfShowCreationAllowed(Screen, Date, Integer) boolean
  - getShowsForScreen(Screen) List~Show~
}
class Theatre {
  + Theatre(String, String) 
  - String id
  - List~Screen~ screens
  - String name
  + addScreen(Screen) void
   List~Screen~ screens
   String name
   String id
}
class TheatreController {
  + TheatreController(TheatreService) 
  + createScreenInTheatre(String, String) String
  + createTheatre(String) String
  + createSeatInScreen(Integer, Integer, String) String
}
class TheatreService {
  + TheatreService() 
  + getSeat(String) Seat
  + createScreenInTheatre(String, Theatre) Screen
  + getTheatre(String) Theatre
  + createSeatInScreen(Integer, Integer, Screen) Seat
  + getScreen(String) Screen
  + createTheatre(String) Theatre
  - createScreen(String, Theatre) Screen
}

Booking "1" *--> "bookingStatus 1" BookingStatus 
Booking  ..>  InvalidStateException : «create»
Booking "1" *--> "seatsBooked *" Seat 
Booking "1" *--> "show 1" Show 
BookingController "1" *--> "bookingService 1" BookingService 
BookingController "1" *--> "showService 1" ShowService 
BookingController "1" *--> "theatreService 1" TheatreService 
BookingService  ..>  BadRequestException : «create»
BookingService "1" *--> "showBookings *" Booking 
BookingService  ..>  Booking : «create»
BookingService  ..>  NotFoundException : «create»
BookingService "1" *--> "seatLockProvider 1" SeatLockProvider 
BookingService  ..>  SeatPermanentlyUnavailableException : «create»
InMemorySeatLockProvider  ..>  SeatLock : «create»
InMemorySeatLockProvider  ..>  SeatTemporaryUnavailableException : «create»
InMemorySeatLockProvider "1" *--> "locks *" Show 
MovieController "1" *--> "movieService 1" MovieService 
MovieService "1" *--> "movies *" Movie 
MovieService  ..>  Movie : «create»
MovieService  ..>  NotFoundException : «create»
PaymentsController "1" *--> "bookingService 1" BookingService 
PaymentsController "1" *--> "paymentsService 1" PaymentsService 
PaymentsService  ..>  BadRequestException : «create»
PaymentsService "1" *--> "bookingFailures *" Booking 
PaymentsService "1" *--> "seatLockProvider 1" SeatLockProvider 
Screen "1" *--> "seats *" Seat 
Screen "1" *--> "theatre 1" Theatre 
SeatAvailabilityService "1" *--> "bookingService 1" BookingService 
SeatAvailabilityService "1" *--> "seatLockProvider 1" SeatLockProvider 
SeatLock "1" *--> "seat 1" Seat 
SeatLock "1" *--> "show 1" Show 
Show "1" *--> "movie 1" Movie 
Show "1" *--> "screen 1" Screen 
ShowController "1" *--> "movieService 1" MovieService 
ShowController "1" *--> "seatAvailabilityService 1" SeatAvailabilityService 
ShowController "1" *--> "showService 1" ShowService 
ShowController "1" *--> "theatreService 1" TheatreService 
ShowService  ..>  NotFoundException : «create»
ShowService  ..>  ScreenAlreadyOccupiedException : «create»
ShowService  ..>  Show : «create»
ShowService "1" *--> "shows *" Show 
Theatre "1" *--> "screens *" Screen 
TheatreController "1" *--> "theatreService 1" TheatreService 
TheatreService  ..>  NotFoundException : «create»
TheatreService  ..>  Screen : «create»
TheatreService "1" *--> "screens *" Screen 
TheatreService "1" *--> "seats *" Seat 
TheatreService  ..>  Seat : «create»
TheatreService "1" *--> "theatres *" Theatre 
TheatreService  ..>  Theatre : «create»
