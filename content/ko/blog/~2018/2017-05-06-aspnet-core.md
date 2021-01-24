---
title: "DB 검색 문제"
date: 2017-04-23

---

{% highlight c%}
  public async Task<Artwork> SearchArtworkToAddArtworkAsync(Exhibition exhibition, SearchArtworkViewModel model)
  {
    if(database.Artworks.Any(a => a.Name == model.Artwork))
    {
            var artwork = await database.Artworks.SingleOrDefaultAsync(a => a.Name == model.Artwork);
            exhibition.Artworks.Add(artwork);
    await database.SaveChangesAsync();

            return artwork;
    }
    else if (database.ArtistProfiles.Any(a => a.ArtistName == model.Artist))
    {
            var artist = await database.ArtistProfiles.SingleOrDefaultAsync(a => a.ArtistName == model.Artist);
            var artwork = await artistprofileservice.AddArtworkAsync(artist, new AddArtworkViewModel{});
    exhibition.Artworks.Add(artwork);
            await database.SaveChangesAsync();

            return artwork;
    }
    else
    {
      //no artist, no artwork
      var artwork = await artistprofileservice.AddArtworkAsync(null, new AddArtworkViewModel {});
      exhibition.Artworks.Add(artwork);
      await database.SaveChangesAsync();

      return artwork;
    }
  }

  public async Task<Artwork> SearchArtworkToAddArtworkAsync(Gallery gallery, SearchArtworkViewModel model)
  {

    if (database.Artworks.Any(a => a.Name == model.Artwork))
    {
      var artwork = await database.Artworks.SingleOrDefaultAsync(a => a.Name == model.Artwork);
              gallery.Artworks.Add(artwork);
      await database.SaveChangesAsync();

      return artwork;
    }
    else if (database.ArtistProfiles.Any(a => a.ArtistName == model.Artist))
    {
      var artist = await database.ArtistProfiles.SingleOrDefaultAsync(a => a.ArtistName == model.Artist);
              var artwork = await artistprofileservice.AddArtworkAsync(artist, new AddArtworkViewModel {});
              gallery.Artworks.Add(artwork);
              await database.SaveChangesAsync();

      return artwork;
    }
    else
    {
      //no artist, no artwork
              var artwork = await artistprofileservice.AddArtworkAsync(null, new AddArtworkViewModel {});
      gallery.Artworks.Add(artwork);
      await database.SaveChangesAsync();

      return artwork;
    }
  }
```
