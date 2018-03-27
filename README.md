# Country_Book
Developed using Ionic 3 and Angular 5

This is a comprehensive Step by step project done by Angular 5 implementation with Ionic 3 to create Mobile App.

First prepare the following tools, framework, and modules:

- Node.js (make sure using ‘Recommended’ version)
- Ionic 3
- Angular 5

Update Ionic to the latest version using this command.
   
   npm install -g ionic@latest
   
1. Create a New Ionic 3 App
ionic start firstproject blank

Go to the project folder
cd ./ionic3-angular5

Run your application
ionic serve -l

2. Create Ionic 3 - Angular 5 Service
ionic g provider rest

That command generates provider or service file with the default imports like this.
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import 'rxjs/add/operator/map';

Replace all imports with the Angular 5 `HttpClient` and new `rxjs` feature.
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs/Observable';
import { map, catchError } from 'rxjs/operators';

Also, replace 'Http' injection in the constructor.
constructor(public http: HttpClient) {
  console.log('Hello RestProvider Provider');
}

Declare URL string variable below class name.
private apiUrl = 'https://restcountries.eu/rest/v2/all';



We will use free RESTful web service for getting countries. Now, create this function below the constructor.

getCountries(): Observable<{}> {
  return this.http.get(this.apiUrl).pipe(
    map(this.extractData),
    catchError(this.handleError)
  );
}

private extractData(res: Response) {
  let body = res;
  return body || { };
}

private handleError (error: Response | any) {
  let errMsg: string;
  if (error instanceof Response) {
    const err = error || '';
    errMsg = `${error.status} - ${error.statusText || ''} ${err}`;
  } else {
    errMsg = error.message ? error.message : error.toString();
  }
  console.error(errMsg);
  return Observable.throw(errMsg);
}


3. Modify Existing Home Page
Now, it's a time to display country list on the Home page. First, we have to modify "src/pages/home/home.ts" replace all codes with this.

import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';
import { RestProvider } from '../../providers/rest/rest';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  countries: any;
  errorMessage: string;

  constructor(public navCtrl: NavController, public rest: RestProvider) {

  }

  ionViewDidLoad() {
    this.getCountries();
  }

  getCountries() {
    this.rest.getCountries()
       .subscribe(
         countries => this.countries = countries,
         error =>  this.errorMessage = <any>error);
  }

}


Next, open and edit "src/pages/home/home.html" then replace all tags inside `<ionic-content>` tag with this.

<ion-content padding>
  <ion-list>
    <ion-item *ngFor="let c of countries">
      <ion-avatar item-left>
        <img src="{{c.flag}}">
      </ion-avatar>
      <h2>{{c.name}}</h2>
      <p>Capital: {{c.capital}}, Region: {{c.region}}</p>
    </ion-item>
  </ion-list>
</ion-content>


4. Run the App in the Browser and the Device
To test in the browser, run again this command.

ionic serve -l



