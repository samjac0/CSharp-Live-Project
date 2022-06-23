# CSharp-Live-Project
After completing the C# course, I participated in a week-long sprint. I was tasked with using C# within the .net framework to assist in developing a theature's website. Specifically, I was assigned to the production team, and tasked to build CRUD functionality for 'Cast Members' of the theater. 

## Table of Contents
- [Crud Functionality and Code-First Approach](#Crud-Functionality-and-Code-First-Approach)
- [CastMember Photo Storage and Display](#CastMember-Photo-Storage-and-Display)

##Readme not fininshed and cloned from previous live project. Please come back within the next two days.

- [CRUD Functionality](#CRUD-Functionality)
- [Connect to API](#Connect-to-API)
- [Beautiful Soup](#Beautiful-Soup)
- [Conclusion](#Conclusion)

## Crud Functionality and Code-First Approach
I created the CastMember class and built CRUD functionality through MVC. 
```cs
   namespace TheatreCMS3.Areas.Prod.Models
    {
    public enum CastPosition
    {
        Actor,
        Director,
        Technician,
        StageManager,
        Other,
    }
    public enum ProductionName
    {
        Wicked,
        Green_Eggs_and_Ham,
        Angels_in_America,
        The_Glass_Menagerie,
    }
    public class CastMember
    {
        public int CastMemberId { get; set; }
        public string Name { get; set; }
        public int? YearJoined { get; set; }
        public CastPosition MainRole { get; set; }
        public string Bio { get; set; }
        public byte[] Photos { get; set; } //Byte Array Photo Storage
        public bool CurrentMember { get; set; }
        public string Character { get; set; }
        public int? CastYearsLeft { get; set; }
        public int? DebutYear { get; set; }
        public ProductionName ProductionTitle { get; set; }
```



## CastMember Photo Storage and Display
After building basic crud, I added photo storage via byte array.

CastMembersController:
```public byte[] convertImage(HttpPostedFileBase uploadedImage)
        {
            byte[] imageArray;
            using (BinaryReader br = new BinaryReader(uploadedImage.InputStream))
            {
                imageArray = br.ReadBytes(uploadedImage.ContentLength);
            }
            return imageArray;
        }
```


Subsequently, I converted it back to an image to be displayed when needed.
```public ActionResult Edit([Bind(Include = "CastMemberId,Name,YearJoined,MainRole,Bio,CurrentMember,Character,CastYearLeft,DebutYear,Photos,ProductionTitle")] CastMember castMember, HttpPostedFileBase uploadedImage)
        {
            if (ModelState.IsValid)
            {
                var photoByte = convertImage(uploadedImage);
                castMember.Photos = photoByte;
                db.Entry(castMember).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(castMember);
        }
```

## CRUD styling
Using the Python package Beautiful Soup, I parsed data from two websites to display a top ten animated series list and a top 100 animated movies list on two newly created templates that can be accessed from both the navbar and homepage.

The following code scrapes the number and titles of the cartoons from Indiewire’s website where they have ranked the best animated series of all time. There are over 60 cartoons on the list and about ten are displayed per page, so I selected the specific page that named the top ten cartoons. The titles are all under "h3" tags in the HTML code.

views.py:
```cs
    def CartoonScrape(request):
        # Create empty list.
        top_cartoons = []
        # Set up BeautifulSoup.
        source = requests.get("https://www.indiewire.com/feature/best-animated-series-all-time-cartoons-anime-tv-1202021835/5/")
        bs = BeautifulSoup(source.content, 'html.parser')
        # Get all the h3 tags under the div 'entry-content' from source site.
        rankings = bs.find('div', class_='entry-content')
        rank = rankings.find_all('h3')
        # For loop through the h3 tags but in reverse order (because they are displayed reversed on the source page).
        for h3 in reversed(rank):
            titles = h3.text
            top_cartoons.append(titles)
        # Delete indexes 8-9 which are irrelevant h3 tags.
        del top_cartoons[8:10]

        context = {'top_cartoons': top_cartoons}
        return render(request, 'Cartoons/Cartoons_rankings.html', context)
```
![Cartoon Rankings](/Images/Rankings_Cartoons.png)

Furthermore, the code below scrapes the ranking/rating/titles of the top 100 animated movies on the Rotten Tomatoes website.

views.py:
```cs
    def MovieScrape(request):
        # Create empty list.
        top_movies = []
        # Set up BeautifulSoup.
        source = requests.get("https://www.rottentomatoes.com/top/bestofrt/top_100_animation_movies/")
        bs = BeautifulSoup(source.content, 'html.parser')
        # Get all the "a" tags under the table 'table' from source site.
        rankings = bs.find('table', class_='table')
        rank = rankings.find_all('tr')
        # For loop that iterates through all the "a" tags.
        for tr in rank:
            titles = tr.text
            top_movies.append(titles)
        # Delete index 0, which is not needed.
        del top_movies[0]

        context = {'top_movies': top_movies}
        return render(request, 'Cartoons/Cartoons_movies.html', context)
```
![Movie Rankings](/Images/Rankings_Movies.png)

## Conclusion
The Python live project provided me with my first full-scale opportunity to utilize project methodologies and gain a detailed understanding of version control. I worked with other students within an Agile framework environment on the Microsoft Azure DevOps platform. I was able to make commits, merges, and push/pulls in real-time while being aware of how to minimize merge conflicts. I participated in daily standup meetings to discuss progress and roadblocks, as well as a retrospective meeting upon completion of the app. I really enjoyed this process and look forward to utilizing everything learned from this sprint in future projects!

Back to [top](#CSharp-Live-Project)
