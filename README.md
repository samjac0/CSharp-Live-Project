# CSharp-Live-Project
After completing the C# course, I participated in a week-long sprint. I was tasked with using C# within the .net framework to assist in developing a theature's website. Specifically, I was assigned to the production team, and tasked to build CRUD functionality for 'Cast Members' of the theater. 

## Table of Contents
- [Crud Functionality and Code-First Approach](#Crud-Functionality-and-Code-First-Approach)
- [CastMember Photo Storage and Display](#CastMember-Photo-Storage-and-Display)
- [CRUD Styling](#CRUD-Styling)

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
Finally, using Razor, Bootstrap and CSS, I added functionality/readibilty to all CRUD pages. Design layout is as was requested by the client.
![Cartoon Rankings](/Images/Rankings_Cartoons.png)
Index snippets (Cards)
```cs
    <div class="card-group row">
                    @foreach (var castMember in group) //creates a card from each model and 'group'(s) them by 'ProductionTitle'
                    {
                        string photo = "";
                        if (castMember.Photos != null) //checking for null
                        {
                            photo = Convert.ToBase64String(castMember.Photos);
                        }
                        string cardEdit = Url.Action("Edit", "CastMembers", new { id = castMember.CastMemberId });
                        string cardDelete = Url.Action("Delete", "CastMembers", new { id = castMember.CastMemberId });
                        string cardDetails = Url.Action("Details", "CastMembers", new { id = castMember.CastMemberId });
                    <div class="card m-1 rounded col-4 px-1" style="max-width:13rem; width: auto;">
                        <div class="text-center">
                            <img src="data:image/png;base64,@photo" class="rounded card-img-top CastMember--Index-Overlay" alt="...">

                            <div class="card-body py-0">
                                <h5 class="card-title text-black text-center pt-1" style="max-width:100%;">@Html.DisplayFor(modelItem => castMember.Name)</h5>
                            </div>
                        </div>
                        <div class="btn-group-sm CastMember--Index-overlayButtonsBlock">
                            <a href="@cardDetails" class="btn btn-sm CastMember--buttons-secondary-color">
                                <i class="fa fa-info-circle"></i>
                                <br />
                                Details
                            </a>
                            <a href="@cardEdit" class="btn CastMember--buttons-secondary-color">
                                <i class="fa fa-file"></i>
                                <br />
                                Edit
                            </a>
                            <a href="@cardDelete" class="btn btn-danger">
                                <i class="fa fa-trash"></i>
                                <br />
                                Delete
                            </a>
```
Index CSS
```cs
    .CastMember--buttons-secondary-color {
    color: #fff;
    background-color: var(--secondary-color);
    border-color: var(--secondary-color);
}


    .CastMember--buttons-secondary-color:hover {
        background-color: var(--secondary-color);
        border-color: var(--secondary-color);
    }


.CastMember--Index-Overlay {
    position: relative;
    transition: all 0.4s ease;
}

    .CastMember--Index-Overlay:hover {
        opacity: 0.5;
        background-color: black;
    }

.CastMember--Index-overlayButtonsBlock {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    text-align: center;
    opacity: 0;
    background: rgb(0,0,0, 0.5);
    transition: all 0.4s ease;
}

    .CastMember--Index-overlayButtonsBlock:before {
        content: ' ';
        /* adjust 'height' to position overlay content vertically */
        display: block;
        height: 30%;
    }

    .CastMember--Index-overlayButtonsBlock:hover {
        opacity: 1;
        transition: all 0.4s ease;
    }

```

## Conclusion
The CSharp project provided me with my first full-scale opportunity to utilize project methodologies and gain a detailed understanding of version control. I worked with other students within an Agile framework environment on the Microsoft Azure DevOps platform. I was able to make commits, merges, and push/pulls in real-time while being aware of how to minimize merge conflicts. I participated in daily standup meetings to discuss progress and roadblocks, as well as a retrospective meeting upon completion of the app. I really enjoyed this process and look forward to utilizing everything learned from this sprint in future projects!

Back to [top](#CSharp-Live-Project)
