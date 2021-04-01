#include<iostream>
#include <ctime>
#include<set>
#include<map>
#include<string>
#include<vector>
#include<utility>
#include <algorithm>
#include <queue>

using namespace std;

// Adding Users  : List of class users
// Adding Movies : List of class movies
// Adding Reviews
// Top n movies in a genre by critic review
// Average Review Year-wise
// Average Review Movie-Wise

// User {Username, Level}
// Movies {Movie-name, YearOfRelease, Genre, Rating}
// Review {Username, MovieName, Rating}  -- // check the level of the person 

class User {
    
    private:

    string username;
    string level;
    int reviewCount ;

    public:
    
    User(){
        username = "";
        level = "";
        reviewCount = 0;
    }

    User( const User & other){
        username = other.username;
        level = other.level;
        reviewCount = other.reviewCount;
    }
    
    User(string name, string lvl, int cnt){
        username = name;
        level = lvl;
        reviewCount = cnt;
    }

    bool operator<(const User &other) const {
        if(username == other.username){
            return reviewCount < other.reviewCount;
        } else {
            return username < other.username;
        }
    }

    string getName(){
        return username;
    }

    string getlevel(){
        return level;
    }

    int getReviewCount(){
        return reviewCount;
    }

    void setReviewCount(int cnt){
        reviewCount = cnt;
        if( cnt > 3 ){
            level = "Critic";
        } else if ( cnt > 20 ){
            level = "Expert";
        } else if ( cnt > 50 ){
            level = "Admin";
        }
    }

};

class Movies {
    
    private:

    string movieName;
    int year;
    vector<string> genre;
    float rating;
    int countOfRatings;

    public:
    
    Movies(){
        movieName = "";
        year = 0;
        genre = {""};
        rating = 0;
        countOfRatings = 0;
    }

    Movies( const Movies & other){
        movieName = other.movieName;
        year = other.year;
        genre = other.genre;
        rating = other.rating;
        countOfRatings = other.countOfRatings;
    }

    Movies(string name, int yr, vector<string> gen, float rate, int cnt ) {
        movieName = name;
        year = yr;
        genre = gen;
        rating = rate;
        countOfRatings = cnt;
    }

    bool operator<(const Movies &other) const {
        if(movieName == other.movieName){
            return rating < other.rating;
        } else {
            return movieName < other.movieName;
        }
    }

    string getMovieName(){
        return movieName;
    }

    float getRating(){
        return rating;
    }

    void updateRating(float newRating){
        float oldRating = rating;
        rating = (((oldRating * countOfRatings) + newRating )/ (countOfRatings + 1));
        countOfRatings += 1;
        cout<<"\nThe updated new rating of the movie : '" << movieName << "' is \t" << rating << "  with " << countOfRatings <<" reviews. "<<endl;
    }

    int getYear(){
        return year;
    }

    vector<string> getGenre(){
        return genre;
    }
};

class MovieReviewSystem {
    
    public:

    map < std::string , User> userData;
    map < std::string , Movies> movieData;

    map<User, pair< Movies, float> > ratingData;
    map<User, pair< Movies, float> > criticData;

    void addUser(string name){
        
        User us(name, "Viewer" , 0);
        userData.insert({name, us});

    }

    void showUserData(){
        
        map<string, User>::iterator it;
        cout << "\t" << "   Name   " <<"\t" << "   Level   " << "\t" << "Count of Ratings" <<endl;
        for(it = userData.begin(); it!= userData.end(); it++){
            User u(it->second);
            cout << "\t   " <<    u.getName()  <<"   \t   " << u.getlevel() << "   \t   " << u.getReviewCount() <<endl;
        }

    }

    void addMovie(string movieName, int year , vector<string> genre){
        
            Movies mov(movieName, year, genre, 0.0 , 0);
            movieData.insert({movieName,mov});
    }

    void showMovieData(){

        map<string, Movies>::iterator it;
        cout << "\t" << "   Name   " <<"\t" << "   Year   " << "\t" << "Ratings" <<endl;
        for(it = movieData.begin(); it!= movieData.end(); it++){
            Movies m(it->second);
            cout << "\t   " <<    m.getMovieName()  <<"   \t   " << m.getYear() << "   \t   " << m.getRating() <<endl;
        }

    }

    void addRating (string username, string movieName , float rating){
        
        bool flagMovieUnreleased = false;
        time_t now = time(0);
        tm *ltm = localtime(&now);
        
        Movies mov(movieData[movieName]);
        if (mov.getYear() >= 2021 ) {
            flagMovieUnreleased = true;
            cout << "\nInvalid Review : Movie not yet released. \n";
        }
        
        if(flagMovieUnreleased == false) {
             bool flagReviewExists = false;
             map<User, pair< Movies,float> > :: iterator it;
             for( it = ratingData.begin() ; it!= ratingData.end() ; it++){
                 User ur(it->first) ;
                 pair<Movies, float > p = it->second;
                 if(ur.getName() == username && p.first.getMovieName() == movieName){
                     cout << "\nThis movie is already reviewd by "<< username <<"!\n";
                     flagReviewExists = true;
                 } 
             }
            
            if(flagReviewExists == false){
                
                User u(userData[username]);
                int cnt = u.getReviewCount();
                u.setReviewCount(cnt+1);
                userData[username] = u;
                Movies m(movieData[movieName]);
                string lvl = u.getlevel();
                
                if( lvl == "Critic" ){
                    rating = 2 * rating;
                    criticData.insert({u,make_pair(m,rating)});
                } else {
                    rating = rating;
                }
        
                ratingData.insert({u,make_pair(m,rating)});
                
                m.updateRating(rating);
                movieData[movieName] = m;
            }    
            
        }    
    }


    float averageMovieRating(string movie){
        
        Movies reqMovie = movieData[movie];
        float averageRating = reqMovie.getRating();
        
        return averageRating;

    }

    float yearWiseAvgRating(int year){
        map<string, Movies>::iterator it;
        float sumRating = 0;
        int countOfMovies = 0;
        for(it = movieData.begin(); it!= movieData.end(); it++){
            Movies mov(it->second);
            if(mov.getYear() == year){
                sumRating += mov.getRating();
                countOfMovies += 1;
            } 
        }

        return sumRating/countOfMovies;
    }

    void topNCriticGenre (int n , string genre) {
        
        map<User, pair< Movies,float> > :: iterator it;
        //min heap;
        priority_queue<pair<int, Movies>, vector<pair<int, Movies>>, greater<pair<int, Movies>> > pq;
        
         for( it = criticData.begin() ; it!= criticData.end() ; it++){
            
             User ur(it->first) ;
             pair< Movies, float > p = it->second;
             Movies mv(p.first);
             vector<string> genreList = mv.getGenre();
             int reviewRate = mv.getRating();
             
             bool isOfGenre;
             if(find(genreList.begin(), genreList.end(), genre) != genreList.end()){
                 isOfGenre = true;
             } else {
                 isOfGenre = false;
             }
           
             if( isOfGenre == true ){
                if(pq.size() < n) {
                    pq.push(make_pair(reviewRate,mv));
                } else {
                    if( reviewRate > pq.top().first ){
                        pq.pop();
                        pq.push(make_pair( reviewRate, mv));
                    }
                }
             }

         }
    
          while (pq.empty() == false)
            {
                Movies mv(pq.top().second);
                cout << mv.getMovieName() << "\n";
                pq.pop();
            }
    
        
    }    

};


int main (){
    
    MovieReviewSystem exampleSystem;
    
    cout<< "Adding Users:\n\n";
    exampleSystem.addUser("SRK");
    exampleSystem.addUser("Salman");
    exampleSystem.addUser("Deepika");
    exampleSystem.showUserData();
    
    cout<<endl<<endl;
    
    vector<string> vec {"Action", "Comedy"} ;
    vector<string> dr {"Drama"};
    vector<string> cm {"Comedy"};
    vector<string> rm {"Romance"};
    
    cout<< "Adding Movies:\n\n";
    exampleSystem.addMovie("Don",2006, vec );
    exampleSystem.addMovie("Tiger", 2008, dr);
    exampleSystem.addMovie("Padmaavat", 2006, cm);
    exampleSystem.addMovie("Lunchbox", 2021, dr);
    exampleSystem.addMovie("Guru", 2006, dr);
    exampleSystem.addMovie("Metro", 2006, rm);

    exampleSystem.showMovieData();
    cout<<endl<<endl;
    
    cout<< "Adding Reviews:\n\n";
    exampleSystem.addRating("SRK", "Don", 2);
    exampleSystem.addRating("SRK", "Padmaavat", 8);
    exampleSystem.addRating("Salman", "Don", 5);
    exampleSystem.addRating("Deepika", "Don", 9);
    exampleSystem.addRating("Deepika", "Guru", 6);
    exampleSystem.addRating("SRK", "Don", 10);
    exampleSystem.addRating("Deepika", "Lunchbox", 5);
    exampleSystem.addRating("SRK", "Tiger", 5);
    exampleSystem.addRating("SRK", "Metro", 7);  // Since SRK is a critic now : the rating will be 2 times (2x)
    
    cout<<"\n\nFinal User Data\n\n";
    exampleSystem.showUserData();
    cout<<endl<<endl;
    cout<<"Final Movie Data\n\n";
    exampleSystem.showMovieData();
    cout<<endl<<endl;
    cout<<"The average review score for the movie Don is "<< exampleSystem.averageMovieRating("Don");
    cout<<endl<<endl;
    cout<<"The average review score for the year 2006 is "<<exampleSystem.yearWiseAvgRating(2006);
    cout<<endl<<endl;
    cout<< "Top 3 "<< " movies under the genre Romance reviewed by critics are: \n";
    exampleSystem.topNCriticGenre(3, "Romance");
    //For now only SRK is the critic and only 1 movie is there under this category!
    cout<<endl<<endl;
    return 0;
}
