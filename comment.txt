# stack-overflow-comment


Hi @TheLearningDev I'm not an expert in this area, so it could well be that I've got something wrong here. 

I made the call to AWS via the pre-signed URL via an API in my NEXT.js application, so that it always comes from the server and not from the client, since the call to AWS can't be made from the client.

on the main site 

const encodedURI = encodeURIComponent(input);
const url = `/api/s3?signedURL=${encodedURI}`;
const response = await axios.get(url, { responseType: 'blob' });

in /api/s3/route.ts

export async function GET(req: NextRequest) {
	const url = new URL(req.url);
	const signedURL = url.searchParams.get('signedURL')
		
	try {
    const response = await axios.get(signedURL, {
      responseType: 'arraybuffer',
    });
    return new Response(response.data, {
      status: 200,
    });
	} catch (error) {
		...
	}
}
