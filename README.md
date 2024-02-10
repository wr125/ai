full stack app (templ, go, echo, htmx, and sqlite3)

package internal_views
import "github.com/wr125/ai/views/db"

templ Data() {
	<select
		id="CustomerDropdown"
		label="Select a Company"
	>
		<option
			value={ "value" }
		></option>
    
		for i,v := range db.GetUserById() {
			<option
				value={ v.CustomerNo }
			>
				{ v.CustomerName }
			</option>
		}
		@attachCustomerDropdownChange()
	</select>
}

script attachCustomerDropdownChange() {
{
  const select = document.getElementById("CustomerDropdown");
  if (!(select instanceof HTMLSelectElement)) {
    return
  }
  select.addEventListener("change", function () {
    select.disabled = true;
    const selectedCustomer = select.selectedOptions[0].value;

    htmx
      .ajax("GET", "/OrderForm/api/get/Customer", {
        swap: "multi:#OrderInfoFormContainer:outerHTML,#CustomerInfoFormContainer:outerHTML",
        values: { customer: selectedCustomer },
      })
      .then(() => {
        select.disabled = false;
        select.focus();
      });
  });
}

}